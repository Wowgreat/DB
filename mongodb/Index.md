# Indexes

## Index Properties

### TTL Indexes

#### Expire Data from Collections by Setting TTL

- Expire Documents after a Specified Number of Seconds

```mongo
db.log_events.createIndex({'field_name': 1}, {expireAfterSeconds: 10})
```

