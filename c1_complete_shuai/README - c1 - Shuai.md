README - c1 - Shuai

### [Section 1 - Relational Database]

Formatted DB example:

salesperson
+------+--------+-------+----------+
| u_id | u_name | u_age | u_salary |
+------+--------+-------+----------+
| 1    | Abe    |    61 |   140000 |
| 2    | Bob    |    34 |    44000 |
| 5    | Chris  |    34 |    40000 |
| 7    | Dan    |    41 |    52000 |
| 8    | Ken    |    57 |   115000 |
| 11   | Joe    |    38 |    38000 |
+------+--------+-------+----------+

customer
+------+----------+----------+--------+
| u_id | u_name   | u_city   | u_type |
+------+----------+----------+--------+
| 4    | Samsonic | Pleasant | J      |
| 6    | Panasung | oaktown  | J      |
| 7    | Samony   | jackson  | B      |
| 9    | Orange   | jackson  | B      |
+------+----------+----------+--------+

shippingorders
+----------+---------------------+---------------+------------------+----------+
| u_number | u_order_date        | u_customer_id | u_salesperson_id | u_amount |
+----------+---------------------+---------------+------------------+----------+
|       10 | 1996-08-02 00:00:00 | 4             | 2                | 540      |
|       20 | 1999-01-30 00:00:00 | 4             | 8                | 1800     |
|       30 | 1995-07-14 00:00:00 | 9             | 1                | 460      |
|       40 | 1998-01-29 00:00:00 | 7             | 2                | 2400     |
|       50 | 1998-02-03 00:00:00 | 6             | 7                | 600      |
|       60 | 1998-03-02 00:00:00 | 6             | 7                | 720      |
|       70 | 1998-05-06 00:00:00 | 9             | 7                | 150      |
+----------+---------------------+---------------+------------------+----------+


Questions:

a) The names of all `salesperson` that do not have an order with `Samsonic`.
b) The names of `salesperson` that have 2 or more orders.
c) The names and ages of all `salesperson` having a salary of `100,000` or greater.
d) When was the earliest and the latest order made to `Samony`.



Answers:

a) select u_name from salesperson where u_id NOT IN(select u_salesperson_id from shippingorders where u_customer_id IN(select u_id from customer where u_name = "Samsonic"); 
b) select s.u_name from salesperson s inner join shippingorders t on s.u_id = t.u_salesperson_id group by t.u_salesperson_id having count(t.u_salesperson_id) >= 2;
c) select u_name, u_age from salesperson where u_salary >=100000;
d) select min(u_order_date) as "earliest", max(u_order_date) as "lastest" from shippingorders where u_customer_id IN(select u_id from customer where u_name = "Samony");


Comments:

a) I used triple nested sub-query.  The first subquery is to isolate the Customer u_id by the given name.  Then the 2nd subquery is to check which salesperson have an Order with the given name, by matching the u_id(primary key) from Customer and the u_customer_id(as the foreign key).  Then the outer most query is to find out which salesperson do have not have an order and display their name match salesperson's primary key(u_id in saleperson table) and foreign key(u_salesperson_id in shippingorders)

There's probably other ways to query this by joining, but I find this to be simple and straight forward.  

b) I have to do group by and having count to be able to count the #'s of the orders by if a salesperson have more than 2 orders
c) This is pretty straight forward to just by able to select the proper columns and display using a condition
d) Min and Max is used here to find the earliest and the latest value, good thing MySQL can sort on its own that's why these math functions work.  Also the subquery is to match the primary/foreign keys from two tables and the given name



### [Section 2 - Non relational Database]

`driver` collection:

```
{
  "_id": ObjectId("5cf6c7dcaacc8b7e7dc29d48"),
  "name": "Johnny Marr",
  "lastSeen": ISODate("2018-12-26T15:57:27.483Z"),
  "vehicleData": {
    "make": "Lexus",
    "model": "ES300",
  }
},
{
  "_id": ObjectId("5cf6c7dcaacc8b7e7dc29d47"),
  "name": "johnny Cash",
  "lastSeen": ISODate("2018-12-02T15:57:27.483Z"),
  "vehicleData": {
    "make": "Toyota",
    "model": "Camry",
  }
},
{
  "_id": ObjectId("58406214aad408103a1c725c"),
  "name": "Anthony Davis",
  "lastSeen": ISODate("2018-11-20T15:57:27.483Z"),
  "vehicleData": {
    "make": "Toyota",
    "model": "Rav4",
  }
},
```
Questions

Given the collection above, answer the following using MongoDB queries:

Questions:

a) Find drivers that was last seen between `2018-11-27` and `2018-12-27`.
b) Find a driver who's first name is `Johnny` and `johnny`.
c) Find the drivers that drives a `Toyota`.

Answers:

a) db.drivers.find( {lastSeen: {$lt: new Date("2018-12-27"), $gt: new Date("2018-11-27")}}).pretty()
b) db.drivers.find( {name: /^Johnny.*|^johnny.*/}).pretty()
c) db.drivers.find( {"vehicleData.make": "Toyota"} ).pretty()

Comments:
I'm new to MongoDB and so there's probably better ways to do these queries.  I have set up an local CentOS MongoDB using the instructions on the MongoDB website, and will attach the reference pages.

First installed MongoDB, by adding the mongoDB repo into my repo list on my CentOS, then it was simple yum install and editing the user by chown, so on so forth.  And I used the Mongo shell by using the mongo client.  

Second, created a DB by doing "use drivers", and apparently if you insert a collection in the database, you can create a db.  I created an "drivers" collection in drivers DB by "db.drivers.insertOne()".  I inserted the 3 documents separately, only to find out i can actually do "db.drivers.insertMany()".  Need to read deeper next time around, would've been alot easier.  

After having inserted the documents, I did an "db.drivers.find({}).pretty()" to print out what my test bed is, so I can test my own use cases.  

From here I was able to sorta construct some queries to satisfy the question condition.  
Links referenced:
https://docs.mongodb.com/manual/reference/command/insert/
https://docs.mongodb.com/manual/tutorial/query-documents/
https://docs.mongodb.com/manual/tutorial/query-embedded-documents/

a) Used less than and greater operator similar to relational database that can used to search for date range.  I found out that type have to be created within the query to be able to search for that field/value pair, which ended up in my query as "new Date()"
b) Used Regex101 to test my regex cases.  Since we're using only first name, assuming it's the first char of the string, that's why I used "^".  Didn't know mongoDB is also case sensitive and exact.  I suppose this is because it's similar to JSON as a text string
c) Had to use the dot notation similar to JSON to u it to search for an nested field/value. This would probably fail if "Toyota" is spelled "toyota"
