### Transaction

> IMPORTANT:
>
> In most cases, multi-document transaction incurs a greater performance cost over single document writes, and the availability of multi-document transaction should not be a replacement for effective schema design. Modeling your data appropriately will minimize the need for multi-document transactions.

```mongo
// Start a session.
session = db.getMongo().startSession( { readPreference: { mode: "primary" } } );

employeesCollection = session.getDatabase("hr").employees;
eventsCollection = session.getDatabase("reporting").events;

// Start a transaction
session.startTransaction( { readConcern: { level: "snapshot" }, writeConcern: { w: "majority" } } );

// Operations inside the transaction
try {
   employeesCollection.updateOne( { employee: 3 }, { $set: { status: "Inactive" } } );
   eventsCollection.insertOne( { employee: 3, status: { new: "Inactive", old: "Active" } } );
} catch (error) {
   // Abort transaction on error
   session.abortTransaction();
   throw error;
}

// Commit the transaction using write concern set at transaction start
session.commitTransaction();

session.endSession();
```

#### Transactions and Sessions

At any given time, you can have at most one open transaction for a session.

>IMPORTANT:
>
>When using the drivers, you must pass the session to each operation in the transaction
>
>To associate read and write operations with a transaction, you **must** pass the session to each operation in the transaction. For examples, see [Transactions in Applications](https://docs.mongodb.com/manual/core/transactions/#transactions-retry).

#### Transaction and mongo Shell

The following mongo shell methods are available for transactions:

```
- Session.startTransaction()
- Session.commitTransaction()
- Session.abortTransaction()
```

#### Transactions and Operations

For transactions:

- You can specify read/write (CRUD) operations on **existing** collections. The collections can be in different databases. For a list of CRUD operations, see [CRUD Operations](https://docs.mongodb.com/manual/core/transactions-operations/#transactions-operations-crud).
- You cannot read/write to collections in the `config`, `admin`, or `local` databases.
- You cannot write to `system.*` collections.
- You cannot return the supported operation’s query plan (i.e. `explain`).

- For cursors created outside of transactions, you cannot call [`getMore`](https://docs.mongodb.com/manual/reference/command/getMore/#dbcmd.getMore) inside a transaction.
- For cursors created in a transaction, you cannot call [`getMore`](https://docs.mongodb.com/manual/reference/command/getMore/#dbcmd.getMore) outside the transaction.
- Creating or dropping a collection or an index, are not allowed in multi-document transactions.