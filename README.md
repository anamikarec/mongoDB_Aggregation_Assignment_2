### Mongo - Aggregation pipelines Pending
#### Problem
##### Data
##### create a mock movies table in sequel and mongodb which has 500 documents which has following columns/fields

- id
- movie_name
- movie_genre
- production_year ( between 1990 to 2020)
- budget ( 9000 to 20000)
- rating ( 0 - 10 )
- rated ( PG or G )
- language
- genres
##### Problem
- create an aggregation query for the following
1. 
- rating is atleast 8
- genres do not contain "Thriller" or "Romance"
- rated "PG"
- languages contain English or French
- production_year after 2012
2. you have been given a task by your manager
- you have a huge collection of data
- you need to find the total number of duplicates that are found on against a key
- here the duplicates are formed from the name
```js
{ name: "aman", id: 1 } , { name: "albert", id : 2 } { name: "aman", id: 3 } , { name: "albert", id : 4 }  { name :"nrupul", id: 5 }
```

- in this case required output is
```js
{ name: "aman", duplicates: 1 }, { name: "albert", duplicates: 1 } ,{ name: "nrupul", duplicates: 0 }
```
- use aggregations and try to solve this
3. 
- you have a set of data in a collection in the following manner
- city
- email
- order_id
- there can be duplicate emails
- you need to filter out the total no of unique emails on each city
- you can use aggregation for this
4. 
- create a database with multiple collectins
- lets assume you are building an ecommerce application and you need to create a system for that
- users collection with all relevant users information: name, email, country, address ( nested object etc )
- all categories
- products: sku, id, item, qty, price with category information
- orders collection with: containing information of user, and purchases made, and all required values, transaction details

1. 
1. 1. 
```js
     db.movie.aggregate( [ { $match: { rating: { $gte: 8 } }}])
```

1. 2. 
```js
    db.movie.aggregate( [ { $match: { genres: { $nin: ['Thriller','Romance'] } }}])
```

1. 3. 
```js
   db.movie.aggregate([{$match:{rated:'PG'}}])
```

1. 4. 
```js
    db.movie.aggregate([{$match:{language : {$in : ['French','English']}}},{$project : {language:1,_id:0}}])
```

1. 5. 
```js
     db.movie.aggregate([{$match:{production_year : {$gt : 2012}}},{$project : {production_year:1,_id:0}}])
```

2.  
```js
   db.newuser.aggregate([{$match : {}},{$group : {_id : '$name',duplicates : {$sum:1}}}])  
```
3. 
```js
    db.city.aggregate([{$group : {_id:'$email',occurence:{$sum : 1}}},{$match : {_id : {$ne : null},'occurence':{$eq:1}}},{$project:{_id : 0,email :'$_id',uniqueFoundMail : '$occurence'}}])
```
