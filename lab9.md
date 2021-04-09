## Checkpoint 0

Here is my paragraph on the MongoDB license change:
https://github.com/roryeiffe/Spoiler-Block/discussions/21

## Checkpoint 1

### Connection Accepted:

![Connection Accepted](check1.1.png)

### Mongo Terminal Working:

![Terminal](check1.2.png)

## Checkpoint 2

### CreateCollection Message:

![Collection](check2.1.png)

## Checkpoint 3

### Commands:

This command returns all words in the database.
```
db.definitions.find()
```

![find](check3.1.png)

This command only returns one word from the database.
```
db.definitions.findOne()
```

These last 2 commands return the object in the database whose word is "Capitaland" and whose id is "56fe9e22bad6b23cde07b8ce" respectively.
```
db.definitions.find({word: "Capitaland"}) 
db.definitions.find({_id: ObjectId("56fe9e22bad6b23cde07b8ce")})
```

![commands](check3.2.png)


### Insert and Update commands:
```
db.definitions.insert({word: "rubber duck", definition: "toy shaped like a stylized duck, generally yellow with a flat base"})

db.definitions.update({  "_id" : ObjectId("56fe9e22bad6b23cde07b8c1"), "definition" : " n. A main staple of the Rensselaer Diet.", "word" : "Beer" }, { "_id" : ObjectId("56fe9e22bad6b23cde07b8c1"), "definition" : " n. This is the alternate beer definition.", "word" : "Beer" })
```

### Find these updated words:
```
db.definitions.find({word: "rubber duck"}) 
db.definitions.find({word: "Beer"})
```

![Changed](check3.3.png)


### Git Diff:

#### Added Rubber duck:

![add](check3.4.png)

#### Removed Old Beer Definition:

![remove](check3.5.png)

#### Updated Beer Definition:

![update](check3.6.png)

## Checkpoint 4

### Script:

```python
from pymongo import MongoClient
from bson.objectid import ObjectId
import pprint
client = MongoClient()

if __name__ == '__main__':
    client = MongoClient('localhost', 27017)
    # Get the database
    db = client.mongo_db_lab
    # Get the collection
    collection = db.definitions
    # Fetch all records:
    for post in collection.find():
    	pprint.pprint(post)
    print()
    # Fetch one record
    pprint.pprint(collection.find_one())
    print()
    # Find specific record:
    pprint.pprint(collection.find_one({"word":"Za"}))
    print()
    # Querying by object id:
    pprint.pprint(collection.find_one({"_id":ObjectId('56fe9e22bad6b23cde07b945')}))
    print()
    # Inserting:
    word = {"word":"rubber duck",
    		"definition": "A rubber duck or rubber ducky is a toy shaped like a stylized duck, generally yellow with a flat base.",
    		"_id":"duck_id3"}
    word_id = collection.insert_one(word).inserted_id
    # print out the word that we inserted:
    pprint.pprint(collection.find_one({"_id": word_id}))
```

### Terminal Output:

You can see the bottom of the fetch_all query. I didn't include the entire thing to save space. There are also a few entries of rubber duck because I made a few mistakes when querying, causing me to reinster with a new id:

![terminal](check4.1.png)


## Checkpoint 5

### Script

```python
from pymongo import MongoClient
client = MongoClient()
import random
import datetime


def random_word_requester():
    '''
    This function should return a random word and its definition and also
    log in the MongoDB database the timestamp that it was accessed.
    '''
    client = MongoClient('localhost', 27017)
    # Get the database
    db = client.mongo_db_lab
    # Get the collection
    collection = db.definitions
    words = []
    # append each word to list
    for post in collection.find():
    	words.append(post)
    # take a random selection:
    selection = random.choice(words)
    # get date:
    d1 = datetime.datetime.utcnow()
    d2 = datetime.datetime.isoformat(d1)
    # get id
    id = selection["_id"]
    # push this date:
    db.definitions.update(
	   { "_id": id},
	   { '$push': { 'dates': d2 } }
	)
    # return the word:
    return selection['word'], selection['definition']


if __name__ == '__main__':
    print(random_word_requester())

```

### Found Capitaland twice:

![capitaland](check5.1.png)

