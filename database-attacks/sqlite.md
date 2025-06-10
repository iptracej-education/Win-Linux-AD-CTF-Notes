# SQLite

### Access to database

```bash
# Access to Audit.db file
sqlite3 <database filename>.db 
sqlite3 Audit.db
```

### View databases

```bash
# Get command
sqlite3> .help

# Check database name
sqlite> .database

# Show tables
sqlite3> .tables

# show the content in the table
sqlite3> select * from <table name>;
```

### View database with GUI&#x20;

```bash
# Open a sqlitebrwoser
sqlitebrowser database.db
```

You can run SQL queries from the application.

<figure><img src="../.gitbook/assets/image (160).png" alt=""><figcaption></figcaption></figure>
