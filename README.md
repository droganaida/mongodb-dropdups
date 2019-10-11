## How to find and delete duplicate documents in MongoDB
### 1. Find duplicate documents in MongoDB
```
db.collection.aggregate([ {$group: { _id: {uniqueField: "$uniqueField"}, uniqueIds: {$addToSet: "$_id"}, count: {$sum: 1} } }, {$match: { count: {"$gt": 1} } } ])
```

### 2. Check if index for unique field exists
```
db.collection.ensureIndex({'uniqueField' : 1}, {unique : true, dropDups : true})
```

### 3. Create temp collection with required index
```
db.temp.createIndex({uniqueField: 1}, {unique: true, dropDups: true})
```

### 4. Copy collection with duplicates to temp collection
```
db.collection.copyTo("temp")
```

### 5. Drop collection with duplicates
```
db.collection.drop()
```

### 6. Create new collection with required index and old name
```
db.collection.createIndex({uniqueField: 1}, {unique: true, dropDups: true})
```

### 7. Copy temp collection to new collection with old name
```
db.temp.copyTo("collection")
```

### 8. Drop temp collection =)
```
db.temp.drop()
```
