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

```
db.definitions.find()
```
This command returns all words in the database.

![find](check3.1.png)

```
db.definitions.findOne()
```
This command only returns one word from the database.

```
db.definitions.find({word: "Capitaland"}) 
db.definitions.find({_id: ObjectId("56fe9e22bad6b23cde07b8ce")})
```
These last 2 commands return the object in the database whose word is "Capitaland" and whose id is "56fe9e22bad6b23cde07b8ce" respectively.

![commands](check3.2.png)
