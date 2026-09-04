---
noteId: "b1208d20a7d111f1bcf2d70766d52b5b"
tags: []

---

# mylinkit — Database

MongoDB with Mongoose — schema conventions, connection management, and query rules.

---

## 1. Stack

| Layer | Choice |
|---|---|
| Database | MongoDB |
| ODM | Mongoose |

Mongoose handles schema definition, validation, and connection pooling.

---

## 2. Connection

A **single cached connection** is reused across the entire application. This ensures:

- In **development**: hot-reloading doesn't leak connections.
- In **production**: the app maintains one healthy connection pool.

The connection helper lives in `/lib/db`:

```
lib/db/
  connect.ts    # cached connection singleton
```

Server Actions and any server-side code import and call this helper to ensure the connection is established before querying.

---

## 3. Models

All Mongoose models live in `/lib/db/models`:

```
lib/db/models/
  user.ts
  link.ts
  ...
```

### Schema conventions

| Setting | Value | Why |
|---|---|---|
| `strict` | `true` | Prevents storing undefined or misspelled fields — rejects bad data at the ODM layer. |
| `timestamps` | `true` | Adds `createdAt` and `updatedAt` automatically — no manual timestamp handling. |

### Indexes

Every model that belongs to a user includes:

- **`userId`** index — all queries are scoped to the signed-in user; this index keeps those lookups fast.
- **`handle`** index — used for public profile lookups (`/[handle]` route).

Add additional indexes as query patterns emerge, but start with these two.

---

## 4. Query Rules

1. **User-scoped only.** Every query includes a `userId` filter matching the signed-in user. No query ever operates across users.
2. **No unscoped queries.** Even admin or utility operations must pass through the same user-scoping guard.
3. **Server Actions are the only mutation path.** All writes go through Server Actions, which enforce auth before running any query.

---

## 5. Adding a new model

1. Create the file in `lib/db/models/<model-name>.ts` (kebab-case).
2. Define the schema with `strict: true` and `timestamps: true`.
3. Add indexes for `userId` and any frequently-queried fields (e.g. `handle`).
4. Export the model using `mongoose.models.<Name> || mongoose.model(...)`.
5. Import and use it in Server Actions or server-side queries only.
