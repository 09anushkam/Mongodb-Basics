# MongoDB  

It is nosqeul database  
It stores data in json like documents  

## Commands  

### Some Basic Commands  

- To open mongodb shell in terminal - `mongosh`  

- To close mongodb shell in terminal - `exit`  

- Clear screen - `cls`  

- To get all databases - `show dbs`  

- To switch to another database - `use databasename`  
(eg:use school)  
(Note - if databasename does not exist already it will create new database for u)  

- To create collection - `db.createCollection("collectionname")`  
eg: db.createCollection("students")  

- To delete a database - `db.dropDatabase()`  

## CRUD  

### Insertion  

- To insert data in collection (string,no.,float,boolean,null) -  
eg: db.collectionname.insertOne({name:"spongebob",age:20,gpa:8,fullTime:true},{name:"sandy",age:19,gpa:null},{name:"Garry",age:21,gpa:6})  

- To insert data in collection containing date -  put `new Date()` in value part of object  
eg: db.collectionname.insertOne({name:"spongebob",age:20,gpa:8,fullTime:true,registerDate:new Date()})  

- To insert multiple value use array in value part -  
eg: db.collectionname.insertOne({name:"spongebob",age:20,gpa:8,fullTime:true,registerDate:new Date(),courses:["calculus","biology]})  

- To insert nested objects -  
eg: db.collectionname.insertOne({name:"spongebob",age:20,gpa:8,fullTime:true,registerDate:new Date(),courses:["calculus","biology],address:{street:"123 Fate St.",city:"NYC",zip:1234}})  

### Read  

- To get data in sorted order -  
db.students.find().sort({name:1})  (acc to name in asc order if u want desc order so instead of 1 use -1)  

- To limit the no. of object that u r getting in return -  
db.students.find().limit(some number)  

- To highest gpa -  
db.students.find().sort({gpa:-1}).limit(1)  

- To lowest gpa -  
db.students.find().sort({gpa:1}).limit(1)  

- To get top 3 students -  
db.students.find().sort({gpa:-1}).limit(3)  

- To get student with name spongebob  
db.students.find({name:"spongebob"})  
(similar to where clause in sql)  

- To return name of all students with ObjectId  
`db.students.find({query},{projection})`  
db.students.find({},{name:1})  
(name:1 or name:true whatever u want and if u don't wangt something use -1 or false)  

### Update  

opertaors used here - $set,$unset,$exits  

- To update fullTime field  
`db.students.find({filter},{update})`  
db.students.updateOne({name:"spongebob"},{$set:{fullTime:false}})  
Note - it is safer if use objectid to update some value  
db.students.find(_id:ObjectId("6318w51738wbbcq"),{$set:{fullTime:false}})  

- To remove a fullTime field  
db.students.find(_id:ObjectId("6318w51738wbbcq"),{$unset:{fullTime:""}})  

- To add fullTime field in everyones document use updateMany  
db.students.updateMany({},{$set:{fullTime:false}})  

- To remove a fullTime field for garry,larry  
db.students.find(name:"larry",{$unset:{fullTime:""}})  
db.students.find(name:"garry",{$unset:{fullTime:""}})  

- Add fullTime true to those who don't have fullTime field  
db.students.updateMany({fullTime:{$exists:false}},{$set:{fullTime:true}})  
eg: larry and garry will have now fullTime:true  

### Delete  

- To delete where name is larry -  
db.students.deleteOne({name:"larry"})  

- To delete where fullTime is false -  
db.students.deleteMany({fullTime:false})  

- To delete where registerDate Exists -  
db.students.deleteMany({registerDate:{$exists:false}})  

## Comparison Operators  

- Not equal $ne  
Find all the students except spongebob  
db.students.find({name:{$ne:"Spongebob"}})  

- Less than $lt  
db.students.find({age:{$lt:20}})  

- Less than or equal to $lte  
db.students.find({age:{$lte:20}})  

- Greater than $gt  
db.students.find({gpa:{$gt:8}})  

- Greater than or equal to $gte  
db.students.find({gpa:{$gte:8}})  

- Range of gpa between 7 and 9  
db.students.find({gpa:{$gte:3,$lte:4}})

- In $in  
db.students.find({name:{$in:["spongebob","sandy","patrick"]}})  

- Not in $nin  
db.students.find({name:{$in:["spongebob","sandy","patrick"]}})  

## Logical Operators  

- and $and  
db.students.find({$and:[{fullTime:true},{age:{$lte:20}}]})  

- or $or  
db.students.find({$or:[{fullTime:true},{age:{$lte:20}}]})  

- nor $nor (fails to match both conditions)  
db.students.find({$nor:[{fullTime:true},{age:{$lte:20}}]})  

- Not $not  
db.students.find({age:{$lt:30}})  (excludes the null value)
db.students.find({age:{$not:{$gte:30}}})  (considers the null value)

## Indexes  

Mongodb uses Btree data structure  

- create index  
db.students.createIndex({name:1})  

- get indexes  
db.students.getIndexes()  

- drop index  
db.students.dropIndex("name_1")

## Collections  

- create collection size is max size here we have considered 10MB and max means max no. of documents  
db.createCollection("teachers",{capped:true,size:10000000,max:100},{autoIndexId:false})  

- db.createCollection("courses")  
db.courses.drop()  

