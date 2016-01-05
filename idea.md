# Project IDEA

## Layers Concept
Database -> Collection -> Document

Database is a group of collections;
Collection is a group of documents;
Document is a single object.

**Document**
```js
var document = cabinet.document('mydocument'); // mydocument.json
document.foo = 'bar'; // automatically save in files with bjson file binding
```
```json
{
   "foo": "bar"
}
```

**Collections**
```js
var collection = cabinet.collection('mycollection'); // mycollection.json
collection.save([{foo: 'bar'}, {bar: 'foo'}]); // create documents
```
```json
{
    "settings": {
        "ai": 2
    },
    "data": [
        {"id": 1, "foo": "bar"},
        {"id": 2, "bar": "foo"}
    ]
}
```

**Database**
```js
var db = cabinet.database('mydb'); // mydb.json
var items = db.collection('items');
items.save([{foo: 'bar'}, {bar: 'foo'}]);
var inventory = db.collection('inventory');
inventory.save({foo: 'bar'});
```

```json
{
    "settings": {},
    "data": {
        "items": {
            "settings": {
                "ai": 2
            },
            "data": [
                {"id": 1, "foo": "bar"},
                {"id": 2, "bar": "foo"}
            ]
        },
        "inventory": {
            "settings": {
                "ai": 1
            },
            "data": [
                {"id": 1, "foo": "bar"}
            ]
        }
    }
}
```
