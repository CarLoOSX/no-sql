# Practica MongoDB

1. ¿Cuantos restaurantes hay en la base de datos? Indica cuantos y como has obtenido el resultado.

```
> db.restaurants.count();
25359
```

2. Busca los restaurantes donde sirvan Tea y otras cosas.

```
NOTE: Without pretty(), is not readable.

db.restaurants.find({cuisine:/Tea/}).pretty();

RESULT : 

{
	"_id" : ObjectId("6065d1708592f18d2b43f42c"),
	"address" : {
		"building" : "26",
		"coord" : [
			-73.9983,
			40.715051
		],
		"street" : "Pell Street",
		"zipcode" : "10013"
	},
	"borough" : "Manhattan",


...
```

3. Busca los restaurantes donde sirvan Tea y otras cosas y no sean Starbucks
   Coffee
```
USING $and & $not:

db.restaurants.find({$and:[{name: {$not: /Starbucks Coffee/}},{cuisine:/Tea/}]}).pretty();
```

4. De la consulta anterior ahora debe devolver solo el name y el borough.

```
INSIDE FIND: {name:1,borough:1} 

db.restaurants.find({$and:[{name: {$not: /Starbucks Coffee/}},{cuisine:/Tea/}]},{name:1,borough:1}).pretty();
{
	"_id" : ObjectId("6065d1708592f18d2b43f42c"),
	"borough" : "Manhattan",
	"name" : "Mee Sum Coffee Shop"
}
{
	"_id" : ObjectId("6065d1708592f18d2b43f44e"),
	"borough" : "Bronx",
	"name" : "Lulu'S Coffee Shop"

```

5. Realiza una consulta que devuelva los restaurantes con puntuaciones
   mayores al 30-11-2014

```
gt:ISODate

db.restaurants.find({grades:{$elemMatch:{date:{$gt:ISODate("2014-11-30T00:00:00Z")} }}}).pretty();
{
	"_id" : ObjectId("6065d1708592f18d2b43f3a5"),
	"address" : {
		"building" : "469",
		"coord" : [
			-73.961704,
			40.662942
		],
		"street" : "Flatbush Avenue",
		"zipcode" : "11225"
	},
	"borough" : "Brooklyn",
	"cuisine" : "Hamburgers",
	"grades" : [
		{
			"date" : ISODate("2014-12-30T00:00:00Z"),
			"grade" : "A",
			"score" : 8
		},
```

6. Realiza una consulta que devuelva el restaurantes con puntuaciones
   mayores al 11-03-2014, tenga una grade igual a “A” y sea de comida
   Chinese.

```
gt:ISODate

db.restaurants.aggregate([{ $match : {grades: { $elemMatch: { date: { $gt:ISODate("2014-03-11T00:00:00Z")},grade : "A" }},cuisine: "Chinese"}}]).pretty();

OR IF YOU WANT NOT ONLY CHINESE OR NOT ONLY GRADE A:

db.restaurants.aggregate([{ $match : {grades: { $elemMatch: { date: { $gt:ISODate("2014-03-11T00:00:00Z")},grade : /A/ }},cuisine:/Chinese/}}]).pretty();

```

7. Realiza una consulta que devuelva el restaurante con puntuaciones
   mayores al 10-01-2013 y que esa puntuación tenga una grade igual a “B” y a su vez esa puntuación tenga un score de 5 y que ese restaurante sea de comida Chinese.

```
db.restaurants.aggregate([{ $match : {grades: { $elemMatch: { date: { $gt:ISODate("2013-01-10T00:00:00Z")}, grade : "B", score: 5 }} ,cuisine:/Chinese/}}]).pretty();

{
	"_id" : ObjectId("6065d1718592f18d2b442070"),
	"address" : {

```

8. Realiza una consulta que muestre los distintos tipos de restaurantes (cuisine) que hay (sin utilizar distinct).

```
GROUP

db.restaurants.aggregate([ { $group : { _id :'$cuisine'} } ]);
{ "_id" : "Hamburgers" }
{ "_id" : "Californian" }
{ "_id" : "Bottled beverages, including water, sodas, juices, etc." }
{ "_id" : "Hawaiian" }


OR GROUP WITH COUNT
db.restaurants.aggregate([ { $group : { _id :'$cuisine', count: { $sum: 1 } } } ]);
{ "_id" : "Hamburgers", "count" : 433 }
{ "_id" : "Californian", "count" : 1 }
{ "_id" : "Bottled beverages, including water, sodas, juices, etc.", "count" : 72 }
{ "_id" : "Hawaiian", "count" : 3 }
{ "_id" : "Ethiopian", "count" : 18 }


```

9. Realiza una consulta que muestre cuantos restaurantes hay en Manhattan por cada tipo de cocina ordenado la cantidad de restaurantes descendientemente.
   
```
db.restaurants.aggregate([{$match:{borough:"Manhattan"}},{$group:{"_id":"$cuisine","number":{$sum:1}}},{$sort:{number:-1}}]);
{ "_id" : "American", "number" : 3205 }
{ "_id" : "Café/Coffee/Tea", "number" : 680 }
{ "_id" : "Italian", "number" : 621 }
{ "_id" : "Chinese", "number" : 510 }

```

10.Realiza una consulta que muestre los 5 restaurantes con la puntuación media (score) más alta.

```

 db.restaurants.aggregate([
...   { "$addFields" : {"avg_score":{"$avg":"$grades.score" }}},
...   { "$sort":{"avg_score":-1}},
...   {$limit:5}
...   ]).pretty();
{
	"_id" : ObjectId("6065d1728592f18d2b4451cb"),
	"address" : {
		"building" : "1068",
		"coord" : [
			-73.958167,
			40.681357
		],
		"street" : "Fulton Street",
		"zipcode" : "11238"
	},
	"borough" : "Brooklyn",
	"cuisine" : "Juice, Smoothies, Fruit Salads",
	"grades" : [
		{
			"date" : ISODate("2015-01-20T00:00:00Z"),
			"grade" : "Not Yet Graded",
			"score" : 75
		}
	],
	"name" : "Juice It Health Bar",
	"restaurant_id" : "50015959",
	"avg_score" : 75
}
{
	"_id" : ObjectId("6065d1728592f18d2b4453f9"),
	"address" : {
		"building" : "2166",
		"coord" : [
			-73.852077,
			40.8339379
		],
		"street" : "Westchester Avenue",
		"zipcode" : "10462"
	},
	"borough" : "Bronx",
	"cuisine" : "Chinese",
	"grades" : [
		{
			"date" : ISODate("2015-01-20T00:00:00Z"),
			"grade" : "Not Yet Graded",
			"score" : 73
		}
	],
	"name" : "Golden Dragon Cuisine",

```
   
11.Realiza una consulta muestra los borough que tienen entre 40 y 60 restaurantes.

```

Hasta aqui todo bien, me hace la cuenta y la muestro de mayor a menor.

db.restaurants.aggregate([
{$group:{"_id":"$borough","total":{$sum:1}}},
{$sort:{total:-1}},
]);

En el momento que añado el match para hacer el between, no funciona.

{$match:{total:{$gt: 40, $lt: 60}}

El resultado de la query sería este:

db.restaurants.aggregate([
{$group:{"_id":"$borough","total":{$sum:1}}},
{$match:{$total:{ $gt: 40, $lt: 60}}
]);

```

12.Cual seria el comando para insertar un nuevo restaurante, con nombre “Pa amb Tomaquet”, con cocina Mediterranea, con un grade creado hoy con puntuación 15 y grade A.
   
```
db.restaurants.insertOne({name: "Pa amb Tomaquet", cuisine: "Mediterranea",grades :{date : new Date(),grade : "A",score : 15} });

{
	"acknowledged" : true,
	"insertedId" : ObjectId("6065ead9b672682ee4e851d2")
}

```
 
13.¿Cuál seria el comando para cambiar todos los restaurantes de comida Italian a Mediterranea?
  
```
db.restaurants.updateMany({ cuisine: "Italian" },{ $set: { cuisine: "Mediterranea" } });

{ "acknowledged" : true, "matchedCount" : 1069, "modifiedCount" : 1069 }

```
  
14.¿Cuál seria el comando para borrar todos los restaurantes de comida Mediterranea?

```
db.restaurants.deleteMany({cuisine:"Mediterranean"});
```

15. ¿Con que comando borrarías toda la colección restaurants?

```
db.restaurants.drop();
```