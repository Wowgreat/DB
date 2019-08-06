### Overview

### Tutorial

#### Making a Connection with MongoClient

```python
from pymongo import MongoClient
client = MongoClient('mongodb://localhost:27017/')
```

#### Getting a Database

```python
db = client.xxxdb	or db = client['xxxdb']
```

#### Getting a Coolection

```python
collection = db.xxxcollection or collection = db['xxxcollection']
```

#### Inserting a Document

```python
posts = db.post
post_id = posts.insert_one(post).inserted_id
```

When a document is inserted a special key, "\_id", is automatically added if the document does not already contain an "\_id" key. The value of "\_id" must be unique across the collection.

#### Update_one

```python
collection.update_one(query={}, newvalues={})
```

#### Set TTL

```python
db_collection.create_index(keys=[(rules.get('field'), 1)], expireAfterSeconds=rules.get('seconds'))
```







