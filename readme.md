# ForerunnerDB - A NoSQL JSON Document DB
ForerunnerDB is developed by [Irrelon Software Limited](http://www.irrelon.com/),
a UK registered company.

## Version 1.3.54

[![npm version](https://badge.fury.io/js/forerunnerdb.svg)](https://www.npmjs.com/package/forerunnerdb)

#### TravisCI Build Test Status
<table>
<tr>
<th>Master</th>
<th>Dev</th>
</tr>
<tr>
<td><img src="https://travis-ci.org/Irrelon/ForerunnerDB.svg?branch=master" title="Master Branch Build Status" /></td>
<td><img src="https://travis-ci.org/Irrelon/ForerunnerDB.svg?branch=dev" title="Dev Branch Build Status" /></td>
</tr>
</table>

## What is ForerunnerDB
ForerunnerDB is a NoSQL JavaScript database with a query language loosly based on
MongoDB (with some differences) and runs on browsers and Node.js.

##### ForerunnerDB & MongoDB
Please see the [differences from MongoDB](#differences-between-forerunnerdb-and-mongodb) section for more information.

## What is ForerunnerDB's Primary Use Case?
ForerunnerDB was created primarily to allow web application developers to easily
store, query and manipulate JSON data in the browser via a simple query language.
It provides the ability to store data passed by an API to the front-end and query
it throughout your application making handling JSON data client-side significantly
easier.

Furthermore, if you use the optional data-binding module, changes to your JSON
data stored in ForerunnerDB are automatically propagated to the DOM. The data
binding system in ForerunnerDB is state-of-the-art, high performance and supports
features like partial updates.

Some web application frameworks provide similar data-binding functionality which is
why data-binding is an optional module that you can choose to include on a per-app
basis as you choose.

Many web applications take data from an API and then represent that on screen
to the user. ForerunnerDB allows you to sort that data and filter it so that you
don't have to make API calls whenever you want data in a different order or
filtered by specific fields and values.

## Demo
You can see an interactive demo at [http://www.forerunnerdb.com/demo.html](http://www.forerunnerdb.com/demo.html)

## Tutorials
[Tutorial 1: A Simple Todo List](http://www.forerunnerdb.com/tutorial/todoList.html)

[Tutorial 2: Simple Chart](http://www.forerunnerdb.com/tutorial/simpleChart.html)

## Download
If you are using Node.js (or have it installed) you can use NPM to download ForerunnerDB via:

```
npm install forerunnerdb
```

This will also work for browser-based development, however if you prefer a more traditional download, please click [here](https://github.com/irrelon/ForerunnerDB/archive/master.zip).

## License
Please see licensing page for latest information: [http://www.forerunnerdb.com/licensing.html](http://www.forerunnerdb.com/licensing.html)

## Browser Compatibility
ForerunnerDB works in all modern browsers (IE8+)

* Android Browser 4
* Blackberry 7
* Chrome 23
* Chrome for Android 32
* Firefox 18
* Firefox for Android 25
* Firefox OS 1.0
* IE 8
* IE Mobile 10
* Opera 15
* Opera Mobile 11
* Phonegap/Apache Cordova 1.2.0
* Safari 4 (includes Mobile Safari)

## Use ForerunnerDB in Browser
Include the fdb-all.min.js file in your HTML (change path to the location you put forerunner):

	<script src="./js/dist/fdb-all.min.js" type="text/javascript"></script>
	
## Use ForerunnerDB in Node.js
After installing via npm (see above) you can require ForerunnerDB in your code:

	var ForerunnerDB = require('forerunnerdb');
	var fdb = new ForerunnerDB();

## Create a Database

	var db = fdb.db('myDatabaseName');

> If you do not specify a database name a randomly generated one is provided instead.

## Collections (Tables)
> Data Binding: Enabled

To create or get a reference to a collection object call (where collectionName is the name of your collection):

	var collection = db.collection('collectionName');

In our examples we will use a collection called "item" which will store some fictitious items for sale:

	var itemCollection = db.collection('item');

### Auto-Creation

When you request a collection that does not yet exist it is automatically created. If
it already exists you are given the reference to the existing collection. If you want
ForerunnerDB to throw an error if a collection is requested that does not already exist
you can pass an option to the *collection()* method instead:

	var collection = db.collection('collectionName', {autoCreate: false});
	
### Specifying a Primary Key Up-Front

On requesting a collection you can specify a primary key that the collection should be
using. For instance to use a property called "name" as the primary key field:

	var collection = db.collection('collectionName', {primaryKey: 'name'});

You can also read or specify a primary key after instantiation via the primaryKey() method.

### Other Patterns

The *collection()* method accepts a number of argument patterns including passing the
primary key as a string in the second argument e.g.

	var collection = db.collection('collectionName', 'name');

> While this will work for legacy reasons it is best to use the options object as shown
above to specify autoCreate and primaryKey option values as using an options object is
considered the "standard".

## Setting Initial Data
When you get a collection instance for the first time it will contain no data. To set data on the collection pass an
array of objects to the setData() method:

	itemCollection.setData([{
		_id: 1,
		name: 'Cat Litter',
		price: 200
	}, {
		_id: 2,
		name: 'Dog Food',
		price: 100
	}]);

Setting data on a collection will empty any existing data from the collection.

You do not *have* to use setData(). You can simply begin inserting data using the
insert() method as soon as you have a collection reference.

## Inserting Documents
You can either insert a single document object:

	itemCollection.insert({
		_id: 3,
		price: 400,
		name: 'Fish Bones'
	});

or pass an array of documents:

	itemCollection.insert([{
		_id: 4,
		price: 267,
		name:'Scooby Snacks'
	}, {
		_id: 5,
		price: 234,
		name: 'Chicken Yum Yum'
	}]);

## Searching the Collection
> **PLEASE NOTE** While we have tried to remain as close to MongoDB's query language
 as possible, small differences are present in the query matching logic. The main
 difference is described here: [Find behaves differently from MongoDB](https://github.com/Irrelon/ForerunnerDB/issues/43)

Much like MongoDB, searching for data in a collection is done using the find() method,
which supports many of the same operators starting with a $ that MongoDB supports. For
instance, finding documents in the collection where the price is greater than 90 but
less than 150, would look like this:

	itemCollection.find({
		price: {
			'$gt': 90,
			'$lt': 150
		}
	});

And would return an array with all matching documents. If no documents match your search, an empty array is returned.

Supported search operators:

* $gt Greater Than
* $gte Greater Than / Equal To
* $lt Less Than
* $lte Less Than / Equal To
* $or Match any of the conditions inside the sub-query
* $and Match all conditions inside the sub-query
* $exists Check that a key exists in the document
* arrayKey.$ Positional selector query
* $elemMatch (projection) - Limit sub-array documents by query
* $elemsMatch (projection) - Mutliple document version of $elemMatch

Searches also support regular expressions for advanced text-based queries. Simply pass the regular expression object as the value for the key you wish to search, just like when using regular expressions with MongoDB.

> See the *Special Considerations* section for details about how names of keys / properties
in a query object can affect a query's operation.

### Projection

#### $elemMatch
The $elemMatch operator limits the contents of an *array* field from the query results to contain only the first element matching the $elemMatch condition.

The $elemMatch operator is specified in the *options* object of the find call rather than
 the query object.
 
[MongoDB $elemMatch Documentation](http://docs.mongodb.org/manual/reference/operator/projection/elemMatch/)
 
##### Usage

	var coll = db.collection('test');
	coll.setData({
		names: [{
			_id: 1,
			text: 'Jim'
		}, {
			_id: 2,
			text: 'Bob'
		}, {
			_id: 3,
			text: 'Bob'
		}, {
			_id: 4,
			text: 'Anne'
		}, {
			_id: 5,
			text: 'Simon'
		}, {
			_id: 6,
			text: 'Uber'
		}]
	});
	
	result = coll.find({}, {
		$elemMatch: {
			names: {
				text: 'Bob'
			}
		}
	});
	
Result is:

	{
		names: [{
			_id: 2,
			text: 'Bob'
		}]
	}

Notice that only the FIRST item matching the $elemMatch clause is returned in the names array.
If you require multiple matches use the ForerunnerDB-specific $elemsMatch operator instead.

#### $elemsMatch
The $elemsMatch operator limits the contents of an *array* field from the query results to contain only the elements matching the $elemMatch condition.

The $elemsMatch operator is specified in the *options* object of the find call rather than
 the query object.
 
##### Usage

	var coll = db.collection('test');
	coll.setData({
		names: [{
			_id: 1,
			text: 'Jim'
		}, {
			_id: 2,
			text: 'Bob'
		}, {
			_id: 3,
			text: 'Bob'
		}, {
			_id: 4,
			text: 'Anne'
		}, {
			_id: 5,
			text: 'Simon'
		}, {
			_id: 6,
			text: 'Uber'
		}]
	});
	
	result = coll.find({}, {
		$elemsMatch: {
			names: {
				text: 'Bob'
			}
		}
	});
	
Result is:

	{
		names: [{
			_id: 2,
			text: 'Bob'
		}, {
			_id: 3,
			text: 'Bob'
		}]
	}

Notice that all items matching the $elemMatch clause are returned in the names array.
If you require match on ONLY the first item use the MongoDB-compliant $elemMatch operator instead.

### Ordering / Sorting Results
You can specify an $orderBy option along with the find call to order/sort your results. This uses the same syntax as MongoDB:

	itemCollection.find({
		price: {
			'$gt': 90,
			'$lt': 150
		}
	}, {
		$orderBy: {
			price: 1 // Sort ascending or -1 for descending
		}
	});

### Limiting Return Fields
You can specify which fields are included in the return data for a query by adding them in
the options object. This follows the same rules specified by MongoDB here: 

[MongoDB Documentation](http://docs.mongodb.org/manual/tutorial/project-fields-from-query-results/)

> Please note that the primary key field will always be returned unless explicitly excluded
from the results via "_id: 0".

#### Usage

	var coll = db.collection('test');
	coll.setData([{
		_id: 1,
		text: 'Jim',
		val: 2131232
	}, {
		_id: 2,
		text: 'Bob',
		val: 2425234321
	}, {
		_id: 3,
		text: 'Bob',
		val: 54353454
	}, {
		_id: 4,
		text: 'Anne',
		val: 1231432
	}, {
		_id: 5,
		text: 'Simon',
		val: 87567455
	}, {
		_id: 6,
		text: 'Uber',
		val: 93472834
	}]);
	
	result = coll.find({}, {
		text: 1
	});
	
Result is:

	[{
		_id: 1,
		text: 'Jim'
	}, {
		_id: 2,
		text: 'Bob'
	}, {
		_id: 3,
		text: 'Bob'
	}, {
		_id: 4,
		text: 'Anne'
	}, {
		_id: 5,
		text: 'Simon'
	}, {
		_id: 6,
		text: 'Uber'
	}]

## Updating the Collection
This is one of the areas where ForerunnerDB and MongoDB are different. By default ForerunnerDB updates only the keys you specify in your update document, rather than outright *replacing* the matching documents like MongoDB does. In this sense ForerunnerDB behaves more like MySQL. In the call below, the update will find all documents where the price is greater than 90 and less than 150 and then update the documents' key "moo" with the value true.

	collection.update({
		price: {
			'$gt': 90,
			'$lt': 150
		}
	}, {
		moo: true
	});

If you want to replace a key's value you can use the $overwrite operator described
in the *Update Operators* section below.

## Quick Updates
You can target individual documents for update by their id (primary key) via a quick helper method:

	collection.updateById(1, {price: 180});

This will update the document with the _id field of 1 to a new price of 180.

### Update Operators
#### $overwrite
The $overwrite operator replaces a key's value with the one passed, overwriting it
completely. This operates the same way that MongoDB's default update behaviour works.

This operator is most useful when updating an array field to a new type such as an object.
By default ForerunnerDB will detect an array and step into the array objects one at a time
and apply the update to each object. When you use $overwrite you can replace the array
instead of stepping into it.

	db.collection('test').update({
		<query>
	}, {
		$overwrite: {
			<field>: <value>
		}
	});

In the following example the "arr" field (initially an array) is replaced by an object:

	db.collection('test').setData({
		_id: "445324",
		arr: [{
			foo: 1
		}]
	});

	db.collection('test').update({
		_id: "445324"
	}, {
		$overwrite: {
			arr: {
				moo: 1
			}
		}
	});
	
	JSON.stringify(db.collection('test').find());

Result:

	[{
		"_id": "445324",
		"arr": {
			"moo": 1
		}
	}]

#### $each
> Version >= 1.3.34

$each allows you to iterate through multiple update operations on the same query result.
Use $each when you wish to execute update operations in sequence or on the same query.
Using $each is slightly more performant than running each update operation one after the
other calling update().

Consider the following sequence of update calls that define a couple of nested arrays and
then push a value to the inner-nested array:

	db.collection('test').setData({
		_id: "445324",
		count: 5
	});
	
	db.collection('test').update({
		_id: "445324"
	}, {
		$cast: {
			arr: "array",
			$data: [{}]
		}
	});
	
	db.collection('test').update({
		_id: "445324"
	}, {
		arr: {
			$cast: {
				secondArr: "array"
			}
		}
	});
	
	db.collection('test').update({
		_id: "445324"
	}, {
		arr: {
			$push: {
				secondArr: "moo"
			}
		}
	});

	JSON.stringify(db.collection('test').find());

Result:

	[
    	{
    		"_id": "445324",
    		"count": 5,
    		"arr": [{"secondArr": ["moo"]}]
    	}
    ]

These calls a wasteful because each update() call must query the collection for matching
documents before running the update against them. With $each you can pass a sequence of
update operations and they will be executed in order:

	db.collection('test').setData({
		_id: "445324",
		count: 5
	});
	
	db.collection('test').update({
		_id: "445324"
	}, {
		$each: [{
			$cast: {
				arr: "array",
				$data: [{}]
			}
		}, {
			arr: {
				$cast: {
					secondArr: "array"
				}
			}
		}, {
			arr: {
				$push: {
					secondArr: "moo"
				}
			}
		}]
	});

	JSON.stringify(db.collection('test').find());

Result:

	[
    	{
    		"_id": "445324",
    		"count": 5,
    		"arr": [{"secondArr": ["moo"]}]
    	}
    ]

As you can see the single sequenced call produces the same output as the multiple update()
calls but will run slightly faster and use fewer resources.

#### $inc
The $inc operator increments / decrements a field value by the given number.

	db.collection('test').update({
		<query>
	}, {
		$inc: {
			<field>: <value>
		}
	});

In the following example, the "count" field is decremented by 1 in the document that
matches the id "445324":

	db.collection('test').setData({
		_id: "445324",
		count: 5
	});
	
	db.collection('test').update({
		_id: "445324"
	}, {
		$inc: {
			count: -1
		}
	});
	
	JSON.stringify(db.collection('test').find());
    
Result:

	[{
		"_id": "445324",
		"count": 4
	}]

Using a positive number will increment, using a negative number will decrement.

#### $push
The $push operator appends a specified value to an array.

	db.collection('test').update({
		<query>
	}, {
		$push: {
			<field>: <value>
		}
	});

The following example appends "Milk" to the "shoppingList" array in the document with the id "23231":

	db.collection('test').setData({
		_id: "23231",
		shoppingList: []
	});
	
	db.collection('test').update({
		_id: "23231"
	}, {
		$push: {
			shoppingList: "Milk"
		}
	});
	
	JSON.stringify(db.collection('test').find());

Result:

	[{
		"_id": "23231",
		"shoppingList": [
			"Milk"
		]
    }]

#### $splicePush
The $splicePush operator adds an item into an array at a specified index.

	db.collection('test').update({
		<query>
	}, {
		$splicePush: {
			<field>: <value>
			$index: <index>
		}
	});

The following example inserts "Milk" to the "shoppingList" array at index 1 in the document with the id "23231":

	db.collection('test').setData({
		_id: "23231",
		shoppingList: [
			"Sugar",
			"Tea",
			"Coffee"
		]
	});
	
	db.collection('test').update({
		_id: "23231"
	}, {
		$splicePush: {
			shoppingList: "Milk",
			$index: 1
		}
	});

	JSON.stringify(db.collection('test').find());

Result:

	[
    	{
    		"_id": "23231",
    		"shoppingList": [
    			"Sugar",
    			"Milk",
    			"Tea",
    			"Coffee"
    		]
    	}
    ]

#### $addToSet
Adds an item into an array only if the item does not already exist in the array.

ForerunnerDB supports the $addToSet operator as detailed in the MongoDB documentation.
Unlike MongoDB, ForerunnerDB also allows you to specify a matching field / path to check
uniqueness against by using the $key property.

In the following example $addToSet is used to check uniqueness against the whole document
being added:

	// Create a collection document
	db.collection('test').setData({
		_id: "1",
		arr: []
	});

	// Update the document by adding an object to the "arr" array
	db.collection('test').update({
		_id: "1"
	}, {
		$addToSet: {
			arr: {
				name: 'Fufu',
				test: '1'
			}
		}
	});

	// Try and do it again... this will fail because a
	// matching item already exists in the array
	db.collection('test').update({
        _id: "1"
    }, {
        $addToSet: {
            arr: {
                name: 'Fufu',
                test: '1'
            }
        }
    });

Now in the example below we specify which key to test uniqueness against:

	// Create a collection document
	db.collection('test').setData({
		_id: "1",
		arr: []
	});

	// Update the document by adding an object to the "arr" array
	db.collection('test').update({
		_id: "1"
	}, {
		$addToSet: {
			arr: {
				name: 'Fufu',
				test: '1'
			}
		}
	});

	// Try and do it again... this will work because the
	// key "test" is different for the existing and new objects
	db.collection('test').update({
        _id: "1"
    }, {
        $addToSet: {
            arr: {
            	$key: 'test',
                name: 'Fufu',
                test: '2'
            }
        }
    });

You can also specify the key to check uniqueness against as an object path such as 'moo.foo'.

#### $pull
The $pull operator removes a specified value or values that match an input query.

	db.collection('test').update({
		<query>
	}, {
		$pull: {
			<arrayField>: <value|query>
		}
	});

The following example removes the "Milk" entry from the "shoppingList" array:

	db.users.update({
		_id: "23231"
	}, {
		$pull: {
			shoppingList: "Milk"
		}
	});

If an array element is an embedded document (JavaScript object), the $pull operator applies its specified query to the element as though it were a top-level object.

#### $move
The $move operator moves an item that exists inside a document's array from one index to another.

	db.collection('test').update({
		<query>
	}, {
		$move: {
			<arrayField>: <value|query>,
			$index: <index>
		}
	});

The following example moves "Milk" in the "shoppingList" array to index 1 in the
document with the id "23231":

	db.users.update({
		_id: "23231"
	}, {
		$move: {
			shoppingList: "Milk"
			$index: 1
		}
	});

#### $cast
> Version >= 1.3.34

The $cast operator allows you to change a property's type within a document. If used to 
cast a property to an array or object the property is set to a new blank array or
object respectively.

This example changes the type of the "val" property from a string to a number:

	db.collection('test').setData({
		val: "1.2"
	});
	
	db.collection('test').update({}, {
		$cast: {
			val: "number"
		}
	});
	
	JSON.stringify(db.collection('test').find());

Result:

	[{
		"_id": "1d6fbf16e080de0",
		"val": 1.2
	}]

You can also use cast to ensure that an array or object exists on a property without
overwriting that property if one already exists:

	db.collection('test').setData({
		_id: "moo",
		arr: [{
			test: true
		}]
	});
	
	db.collection('test').update({
		_id: "moo"
	}, {
		$cast: {
			arr: "array"
		}
	});
	
	JSON.stringify(db.collection('test').find());

Result:

	[{
		"_id": "moo",
		"arr": [{
			"test": true
		}]
	}]

Should you wish to initialise an array or object with specific data if the property is
not currently of that type rather than initialising as a blank array / object, you can 
specify the data to use by including a $data property in your $cast operator object:

	db.collection('test').setData({
		_id: "moo"
	});
	
	db.collection('test').update({
		_id: "moo"
	}, {
		$cast: {
			orders: "array",
			$data: [{
				initial: true
			}]
		}
	});
	
	JSON.stringify(db.collection('test').find());

Result:
	
	[{
		"_id": "moo",
		"orders":[{
			"initial": true
		}]
	}]

#### Array Positional (.$)
Often you want to update a sub-document stored inside an array. You can use the array positional
operator to tell ForerunnerDB that you wish to update a sub-document that matches your query
clause.

The following example updates the sub-document in the array *"arr"* with the _id *"foo"* so
that the *"name"* property is set to *"John"*:

	db.collection('test').setData({
		_id: '2',
		arr: [{
			_id: 'foo',
			name: 'Jim'
		}]
	});
	
	var result = db.collection('test').update({
		_id: '2',
		"arr": {
			"_id": "foo"
		}
	}, {
		"arr.$": {
			name: 'John'
		}
	});

Internally this operation checks the update for property's ending in ".$" and then looks
at the query part of the call to see if a corresponding clause exists for it. In the example
above the "arr.$" property in the update part has a corresponding "arr" in the query part
which determines which sub-documents are to be updated based on if they match or not.

## Get Data Item By Reference
JavaScript objects are passed around as references to the same object. By default when you query ForerunnerDB it will "decouple" the results from the internal objects stored in the collection. If you would prefer to get the reference instead of decoupled object you can specify this in the query options like so:

	var result = db.collection('item').find({}, {
		$decouple: false
	});

If you do not specify a decouple option, ForerunnerDB will default to true and return decoupled objects.

Keep in mind that if you switch off decoupling for a query and then modify any object returned, it will also modify the internal object held in ForerunnerDB, which could result in incorrect index data as well as other anomalies.

## Primary Keys
If your data uses different primary key fields from the default "_id" then you need to tell the collection. Simply call
the primaryKey() method with the name of the field your primary key is stored in:

	collection.primaryKey('itemId');

When you change the primary key field name, methods like updateById will use this field automatically instead of the
default one "_id".

## Removing Documents
Removing is as simple as doing a normal find() call, but with the search for docs you want to remove. Remove all
documents where the price is greater than or equal to 100:

	collection.remove({
		price: {
			'$gte': 100
		}
	});

### Joins
Sometimes you want to join two or more collections when running a query and return a single document with all the data you need from those multiple collections. ForerunnerDB supports collection joins via a simple options key "$join". For instance, let's setup a second collection called "purchase" in which we will store some details about users who have ordered items from the "item" collection we initialised above:

	var fdb = new ForerunnerDB(),
		db = fdb.db('test'),
		itemCollection = db.collection('item'),
		purchaseCollection = db.collection('purchase');

	itemCollection.insert([{
        _id: 1,
        name: 'Cat Litter',
        price: 200
    }, {
        _id: 2,
        name: 'Dog Food',
        price: 100
    }, {
        _id: 3,
        price: 400,
        name: 'Fish Bones'
    }, {
		_id: 4,
		price: 267,
		name:'Scooby Snacks'
	}, {
		_id: 5,
		price: 234,
		name: 'Chicken Yum Yum'
	}]);

	purchaseCollection.insert([{
		itemId: 4,
		user: 'Fred Bloggs',
		quantity: 2
	}, {
		itemId: 4,
		user: 'Jim Jones',
		quantity: 1
	}]);

Now, when we find data from the "item" collection we can grab all the users that ordered that item as well and store them in a key called "purchasedBy":

	itemCollection.find({}, {
		'$join': [{
			'purchase': {
				'itemId': '_id',
				'$as': 'purchasedBy',
				'$require': false,
				'$multi': true
			}
		}]
	});

The "$join" key holds an array of joins to perform, each join object has a key which
denotes the collection name to pull data from, then matching criteria which in this
case is to match purchase.itemId with the item._id. The three other keys are special
operations (start with $) and indicate:

* $as tells the join what object key to store the join results in when returning the document
* $require is a boolean that denotes if the join must be successful for the item to be returned in the final find result
* $multi indicates if we should match just one item and then return, or match multiple items as an array

The result of the call above is:

	[{
		"_id":1,
		"name":"Cat Litter",
		"price":200,
		"purchasedBy":[]
	},{
		"_id":2,
		"name":"Dog Food",
		"price":100,
		"purchasedBy":[]
	},{
		"_id":3,
		"price":400,
		"name":"Fish Bones",
		"purchasedBy":[]
	},{
		"_id":4,
		"price":267,
		"name":"Scooby Snacks",
		"purchasedBy": [{
			"itemId":4,
			"user":"Fred Bloggs",
			"quantity":2
		}, {
			"itemId":4,
			"user":"Jim Jones",
			"quantity":1
		}]
	},{
		"_id":5,
		"price":234,
		"name":"Chicken Yum Yum",
		"purchasedBy":[]
	}]

## Triggers
> Version >= 1.3.12

ForerunnerDB currently supports triggers for inserts and updates at both the
*before* and *after* operation phases. Triggers that fire on the *before* phase can
also optionally modify the operation data and actually cancel the operation entirely
allowing you to provide database-level data validation etc.

Setting up triggers is very easy.

### Example 1: Cancel Operation Before Insert Trigger 
Here is an example of a *before insert* trigger that will cancel the insert
operation before the data is inserted into the database:

	var fdb = new ForerunnerDB(),
		db = fdb.db('test'),
    	collection = db.collection('test');
    
    collection.addTrigger('myTrigger', db.TYPE_INSERT, db.PHASE_BEFORE, function (operation, oldData, newData) {
    	// By returning false inside a "before" trigger we cancel the operation
    	return false;
    });
    
    collection.insert({test: true});

The trigger method passed to addTrigger() as parameter 4 should handle these
arguments:

|Argument|Data Type|Description|
|--------------|---------|-----------------------------------------------------|
|operation|object|Details about the operation being executed. In *before update* operations this also includes *query* and *update* objects which you can modify directly to alter the final update applied.|
|oldData|object|The data before the operation is executed. In insert triggers this is always a blank object. In update triggers this will represent what the document that *will* be updated currently looks like. You cannot modify this object.|
|newData|object|The data after the operation is executed. In insert triggers this is the new document being inserted. In update triggers this is what the document being updated *will* look like after the operation is run against it. You can update this object ONLY in *before* phase triggers.|

### Example 2: Modify a Document Before Update
In this example we insert a document into the collection and then update it afterwards.
When the update operation is run the *before update* trigger is fired and the
document is modified before the update is applied. This allows you to make changes to
an operation before the operation is carried out.

	var fdb = new ForerunnerDB(),
		db = fdb.db('test'),
    	collection = db.collection('test');
    
    collection.addTrigger('myTrigger', db.TYPE_UPDATE, db.PHASE_BEFORE, function (operation, oldData, newData) {
    	newData.updated = String(new Date());
    });
    
    // Insert a document with the property "test" being true
    collection.insert({test: true});
    
    // Now update that document to set "test" to false - this
    // will fire the trigger code registered above and cause the
    // final document to have a new property "updated" which
    // contains the date/time that the update occurred on that
    // document
    collection.update({test: true}, {test: false});
    
    // Now inspect the document and it will show the "updated"
    // property that the trigger added!
    console.log(collection.find());

> Please keep in mind that you can only modify a document's data during a *before*
phase trigger. Modifications to the document during an *after* phase trigger will
simply be ignored and will not be applied to the document. This applies to insert
and update trigger types. Remove triggers cannot modify the document at any time.

### Enabling / Disabling Triggers
> Version >= 1.3.31

#### Enabling a Trigger
You can enable a previously disabled trigger or multiple triggers using the enableTrigger()
method on a collection.

> If you specify a type or type and phase and do not specify an ID the method will
affect all triggers that match the type / phase.

##### Enable a Trigger via Trigger ID

	db.collection('test').enableTrigger('myTriggerId');

##### Enable a Trigger via Type

	db.collection('test').enableTrigger(db.TYPE_INSERT);

##### Enable a Trigger via Type and Phase

	db.collection('test').enableTrigger(db.TYPE_INSERT, db.PHASE_BEFORE);

##### Enable a Trigger via ID, Type and Phase

	db.collection('test').enableTrigger('myTriggerId', db.TYPE_INSERT, db.PHASE_BEFORE);

#### Disabling a Trigger
You can temporarily disable a trigger or multiple triggers using the disableTrigger()
method on a collection.

> If you specify a type or type and phase and do not specify an ID the method will
affect all triggers that match the type / phase.

##### Disable a Trigger via Trigger ID

	db.collection('test').disableTrigger('myTriggerId');

##### Disable a Trigger via Type

	db.collection('test').disableTrigger(db.TYPE_INSERT);

##### Disable a Trigger via Type and Phase

	db.collection('test').disableTrigger(db.TYPE_INSERT, db.PHASE_BEFORE);

##### Disable a Trigger via ID, Type and Phase

	db.collection('test').disableTrigger('myTriggerId', db.TYPE_INSERT, db.PHASE_BEFORE);

## Indices & Performance
ForerunnerDB currently supports basic indexing for performance enhancements when
querying a collection. You can create an index on a collection using the
ensureIndex() method. ForerunnerDB will utilise the index that most closely matches
the query you are executing. In the case where a query matches multiple indexes
the most relevant index is automatically determined. Let's setup some data to index:

	var fdb = new ForerunnerDB(),
		db = fdb.db('test'),
		names = ['Jim', 'Bob', 'Bill', 'Max', 'Jane', 'Kim', 'Sally', 'Sam'],
		collection = db.collection('test'),
		tempName,
		tempAge,
		i;

	for (i = 0; i < 100000; i++) {
		tempName = names[Math.ceil(Math.random() * names.length) - 1];
		tempAge = Math.ceil(Math.random() * 100);

		collection.insert({
			name: tempName,
			age: tempAge
		});
	}

You can see that in our collection we have some random names and some random ages. If we ask Forerunner to explain the query plan for querying the name and age fields:

	collection.explain({
		name: 'Bill',
        age: 17
	});

The result shows that the largest amount of time was taken in the "tableScan" step:

	{
		analysis: Object,
		flag: Object,
		index: Object,
		log: Array[0],
		operation: "find",
		results: 128, // Will vary depending on your random entries inserted earlier
		steps: Array[4] // Lists the steps Forerunner took to generate the results
			[0]: Object
				name: "analyseQuery",
				totalMs: 0
			[1]: Object
				name: "checkIndexes",
                totalMs: 0
			[2]: Object
				name: "tableScan",
                totalMs: 54
			[3]: Object
				name: "decouple",
                totalMs: 1,
        time: Object
	}

From the explain output we can see that a large amount of time was taken up doing a table scan. This means that the database had to scan through every item in the collection and determine if it matched the query you passed. Let's speed this up by creating an index on the "name" field so that lookups against that field are very fast. In the index below we are indexing against the "name" field in ascending order, which is what the 1 denotes in name: 1. If we wish to index in descending order we would use name: -1 instead.

	collection.ensureIndex({
		name: 1
	});

The collection now contains an ascending index against the name field. Queries that check against the name field will now be optimised:

	collection.explain({
		name: 'Bill',
		age: 17
	});

Now the explain output has some different results:

	{
		analysis: Object,
		flag: Object,
		index: Object,
		log: Array[0],
		operation: "find",
		results: 128, // Will vary depending on your random entries inserted earlier
		steps: Array[6] // Lists the steps Forerunner took to generate the results
			[0]: Object
				name: "analyseQuery",
				totalMs: 1
			[1]: Object
				name: "checkIndexes",
                totalMs: 1
			[2]: Object
				name: "checkIndexMatch: name:1",
                totalMs: 0
			[3]: Object
				name: "indexLookup",
                totalMs: 0,
			[4]: Object
				name: "tableScan",
                totalMs: 13,
			[5]: Object
				name: "decouple",
                totalMs: 1,
        time: Object
	}

The query plan shows that the index was used because it has an "indexLookup" step, however we still have a "tableScan" step that took 13 milliseconds to execute. Why was this? If we delve into the query plan a little more by expanding the analysis object we can see why:

	{
		analysis: Object
			hasJoin: false,
			indexMatch: Array[1]
				[0]: Object
					index: Index,
					keyData: Object
						matchedKeyCount: 1,
						totalKeyCount: 2,
						matchedKeys: Object
							age: false,
							name: true
					lookup: Array[12353]
			joinQueries: Object,
			options: Object,
			queriesJoin: false,
			queriesOn: Array[1],
			query: Object
		flag: Object,
		index: Object,
		log: Array[0],
		operation: "find",
		results: 128, // Will vary depending on your random entries inserted earlier
		steps: Array[6] // Lists the steps Forerunner took to generate the results
        time: Object
	}

In the selected index to use (indexMatch[0]) the keyData shows that the index only matched 1 out of the 2 query keys.

In the case of the index and query above, Forerunner's process will be:

* Query the index for all records that match the name "Bill" (very fast)
* Iterate over the records from the index and check each one for the age 17 (slow)

This means that while the index can be used, a table scan of the index is still required. We can make our index better by using a compound index:

	collection.ensureIndex({
		name: 1,
		age: 1
	});

With the compound index, Forerunner can now pull the matching record right out of the hash table without doing a data scan which is very very fast:

	collection.explain({
		name: 'Bill',
		age: 17
	});

Which gives:

	{
		analysis: Object,
		flag: Object,
		index: Object,
		log: Array[0],
		operation: "find",
		results: 128, // Will vary depending on your random entries inserted earlier
		steps: Array[7] // Lists the steps Forerunner took to generate the results
			[0]: Object
				name: "analyseQuery",
				totalMs: 0
			[1]: Object
				name: "checkIndexes",
                totalMs: 0
			[2]: Object
				name: "checkIndexMatch: name:1",
                totalMs: 0
			[3]: Object
				name: "checkIndexMatch: name:1_age:1",
                totalMs: 0,
			[4]: Object
				name: "findOptimalIndex",
                totalMs: 0,
			[5]: Object
				name: "indexLookup",
                totalMs: 0,
			[6]: Object
				name: "decouple",
                totalMs: 0,
        time: Object
	}

Now we are able to query 100,000 records instantly, requiring zero milliseconds to return the results.

Examining the output from an explain() call will provide you with the most insight into how the query
was executed and if a table scan was involved or not, helping you to plan your indices accordingly.

Keep in mind that indices require memory to maintain and there is always a trade-off between
speed and memory usage.

## Data Persistence (Save and Load Between Pages)
Data persistence allows your database to survive the browser being closed, page reloads and navigation
away from the current url. When you return to the page your data can be reloaded.

> Persistence calls are async so a callback should be passed to ensure the operation has completed before
relying on data either being saved or loaded.

Persistence is handled by a very simple interface in the Collection class. You can save the current state
of any collection by calling:

    collection.save(function (err) {
    	if (!err) {
    		// Save was successful
    	}
    });

You can then load the collection's data back again via:

    collection.load(function (err) {
		if (!err) {
			// Load was successful
		}
	});

If you call collection.load() when your application starts and collection.save() when you make changes
to your collection you can ensure that your application always has up-to-date data.

### Manually Specifying Storage Engine
If you would like to manually specify the storage engine that ForerunnerDB will use you can call the
driver() method:

#### IndexedDB

	var fdb = new ForerunnerDB(),
		db = fdb.db('test');
	db.persist.driver('IndexedDB');

#### WebSQL

	var fdb = new ForerunnerDB(),
		db = fdb.db('test');
	db.persist.driver('WebSQL');

#### LocalStorage

	var fdb = new ForerunnerDB(),
		db = fdb.db('test');
	db.persist.driver('LocalStorage');

## Storing Arbitrary Key/Value Data
Sometimes it can be useful to store key/value data on a class instance such as the core db
class or a collection or view instance. This can later be retrieved somewhere else in your
code to provide a quick and easy data-store across your application that is outside of the
main storage system of ForerunnerDB, does not persist, is not indexed or maintained and will
be destroyed when the supporting instance is dropped.

To use the store, simply call the store() method on a collection or view:
 
	var fdb = new ForerunnerDB(),
		db = fdb.db('test');
	db.collection('myColl').store('myKey', 'myVal');

You can then lookup the value at a later time:

	var value = db.collection('myColl').store('myKey');
	console.log(value); // Will output "myVal"

You can also remove a key/value from the store via the unStore() method:

	db.collection('myColl').unStore('myKey');

## Grid / Table Output
> Data Binding: Enabled

ForerunnerDB 1.3 includes a grid / table module that allows you to output data from a collection or view to
an HTML table that can be sorted and is data-bound so the table will react to changes in the underlying
data inside the collection / view.

#### Prerequisites
* The AutoBind module must be loaded

#### Grid Template

Grids work via a jsRender template that describes how your grid should be rendered to the browser. An
example template called "gridTable" looks like this:

	<script type="text/x-jsrender" id="gridTable">
		<table class="gridTable">
			<thead class="gridHead">
				<tr>
					<td data-grid-sort="firstName">First Name</td>
					<td data-grid-sort="lastName">Last Name</td>
					<td data-grid-sort="age">Age</td>
				</tr>
			</thead>
			<tbody class="gridBody">
				{^{for gridRow}}
				<tr data-link="id{:_id}">
					<td>{^{:firstName}}</td>
					<td>{^{:lastName}}</td>
					<td>{^{:age}}</td>
				</tr>
				{^{/for}}
			</tbody>
			<tfoot>
				<tr>
					<td></td>
					<td></td>
					<td></td>
				</tr>
			</tfoot>
		</table>
	</script>

You'll note that the main body section of the table has a *for-loop* looping over the special gridRow
array. This array is the data inside your collection / view that the grid has been told to read from
and is automatically passed to your template by the grid module. Use this array to loop over and
output the row data for each row in your collection.
  
#### Creating a Grid
First you need to identify a target element that will contain the rendered grid:

	<div id="myGridContainer"></div>

You can create a grid on screen via the .grid() method, passing it your target jQuery selector as a
string:

	// Create our instances
	var fdb = new ForerunnerDB(),
		db = fdb.db('test'),
		coll = db.collection('testGrid'),
		grid;
	
	// Insert some data into our collection
	coll.insert({
		firstName: 'Fred',
		lastName: 'Jones',
		age: 15
	});
	
	// Create a grid from the collection using the template we defined earlier
	coll.grid('#myGridContainer', '#gridTable');

#### Auto-Sorting Tools
The table can automatically handle sort requests when a column header is tapped/clicked on.
To enable this functionality simply add the *data-grid-sort="{column name}"* attribute
to elements you wish to use as sort elements. A good example is to use the table column
header for sorting and you can see the correct usage above in the HTML of the table
template.

## Views
> Data Binding: Enabled
A view is a queried subset of a collection that is automatically updated whenever the
underlying collection is altered. Views are accessed in the same way as a collection and
contain all the main CRUD functionality that a collection does. Inserting or updating on
a view will alter the underlying collection.

#### Instantiating a View
Views are instantiated the same way collections are:

	var myView = db.view('myView');

#### Specify an Underlying Data Source
You must tell a view where to get it's data from using the *from()* method. Views can
 use collections and other views as data sources:
 
	var myCollection = db.collection('myCollection');
	
	myCollection.setData([{
		name: 'Bob',
		age: 20
	}, {
		name: 'Jim',
		age: 25
	}, {
		name: 'Bill',
		age: 30
	}]);
	
	myView.from(myCollection);

#### Setting a View's Query
Since views represent live queried data / subsets of the underlying data source they
usually take a query:

	myView.query({
		age: {
			$gt: 24
		}
	});

Using the collection data as defined in myCollection above, a call to the view's *find()*
 method will result in returning only records in myCollection whose age property is greater
 than 24:

	myView.find();
	
Result:

	[{
		"name": "Jim",
		"age": 25,
		"_id": "2aee6ba38542220"
	}, {
		"name": "Bill",
		"age": 30,
		"_id": "2d3bb2f43da7aa0"
	}]

## Overviews
> Data Binding: Enabled

The Overview class provides the facility to run custom logic against the data from
multiple data sources (collections and views for example) and return a single object /
value. This is especially useful for scenarios where a summary of data is required such
as a shopping basket order summary that is updated in realtime as items are added to
the underlying cart collection, a count of some values etc.

Consider a page with a shopping cart system and a cart summary which shows the number
of items in the cart and the total cart value. Let's start by defining our cart
collection:

	var cart = db.collection('cart');

Now we add some data to the cart:

	cart.insert([{
		name: 'Cat Food',
		price: 12.99,
		quantity: 2
	}, {
		name: 'Dog Food',
		price: 18.99,
		quantity: 3
	}]);

Now we want to display a cart summary with number of items and the total cart price, so
we create an overview:

	var cartSummary = db.overview('cartSummary');

We need to tell the overview where to read data from:

	cartSummary.from(cart);

Now we give the overview some custom logic that will do our calculations against the data
 in the cart collection and return an object with our item count and price total:

	cartSummary.reduce(function () {
		var obj = {},
			items = this.find(), // .find() on an overview runs find() against underlying collection
			total = 0,
			i;

		for (i = 0; i < items.length; i++) {
			total += items[i].price * items[i].quantity;
		}

		obj.count = items.length;
		obj.total = total;

		return obj;
	});

You can execute the overview's reduce() method and get the result via the exec() method:

	cartSummary.exec();

Result:

	{count: 2, total: 31.979999999999997}

## Data Binding
>Data binding is an optional module that is included via the fdb-autobind.min.js file.
If you wish to use data-binding please ensure you include that file in your page after
the main fdb-all.min.js file.

The database includes a useful data-binding system that allows your HTML to be
automatically updated when data in the collection changes.

> Binding a template to a collection will render the template once for each document in the
collection. If you need an array of the entire collection passed to a single template see
the section below on *wrapping data*.

Here is a simple example of a data-bind that will keep the list of items up-to-date
if you modify the collection:

### Prerequisites
* Data-binding requires jQuery to be loaded
* The AutoBind module must be loaded

### HTML
	<ul id="myList">
	</ul>
	<script id="myLinkFragment" type="text/x-jsrender">
		<li data-link="id{:_id}">{^{:name}}</li>
	</script>

### JS
	var fdb = new ForerunnerDB(),
		db = fdb.db('test'),
		collection = db.collection('test');

	collection.link('#myList', '#myLinkFragment');

Now if you execute any insert, update or remove on the collection, the HTML will
automatically update to reflect the
changes in the data.

Note that the selector string that a bind uses can match multiple elements, allowing
you to bind against multiple sections of the page with the same data. For instance,
instead of binding against an ID (e.g. #myList) you could bind against a class:

### HTML
	<ul class="myList">
	</ul>
	
	<ul class="myList">
	</ul>

	<script id="myLinkFragment" type="text/x-jsrender">
        <li data-link="id{:_id}">{^{:name}}</li>
    </script>
	
### JS
	collection.link('#myList', '#myLinkFragment');

The result of this is that both UL elements will get data binding updates when the
underlying data changes.

## Bespoke / Runtime Templates
You can provide a bespoke template to the link method in the second argument by passing
an object with a *template* property:

	var fdb = new ForerunnerDB(),
		db = fdb.db('test');
		
	db.collection('test').setData([{
		name: 'Jim'
	}, {
		name: 'Bob'
	}]);
	
	db.collection('test').link('#myTargetElement', {
		template: '<div>{^{:name}}</div>'
	});

This allows you to specify a template programmatically rather than defining your template
as a static piece of HTML on your page.

## Wrapping Data
Sometimes it is useful to provide data from a collection or view in an array form to the
template. You can wrap all the data inside a property via the $wrap option passed to the
link method like so:

	var fdb = new ForerunnerDB(),
		db = fdb.db('test');
		
	db.collection('test').setData([{
		name: 'Jim'
	}, {
		name: 'Bob'
	}]);
	
	db.collection('test').link('#myTargetElement', {
		template: '<ul>{^{for items}}<li>{^{:name}}</li>{{/for}}</ul>'
	}, {
		$wrap: 'items'
	});

Setting the $wrap option to 'items' passes the entire collection's data array into the
template inside the *items* property which can then be accessed and iterated through like
a normal array of data.

You can also wrap inside a ForeunnerDB Document instance which will allow you to control
other properties on the wrapper and have them update in realtime if you are using the
data-binding module.

To wrap inside a document instance, pass the document in the $wrapIn option:

	var fdb = new ForerunnerDB(),
		db = fdb.db('test'),
		doc;
		
	db.collection('test').setData([{
		name: 'Jim'
	}, {
		name: 'Bob'
	}]);
	
	doc = db.document('myWrapperDoc');
	
	doc.setData({
		loading: true
	});
	
	db.collection('test').link('#myTargetElement', {
		template: '{^{if !loading}}<ul>{^{for items}}<li>{^{:name}}</li>{{/for}}</ul>{{/if}}'
	}, {
		$wrap: 'items',
		$wrapIn: doc
	});
	
	doc.update({}, {loading: false});

## Highcharts: Charts & Visualisations
> Data Binding: Enabled

ForerunnerDB can utilise the popular Highcharts JavaScript library to generate charts from collection data
and automatically keep the charts in sync with changes to the collection.

### Prerequisites
The Highcharts JavaScript library is required to use the ForerunnerDB Highcharts module. You can
get Highcharts from http://www.highcharts.com

### Usage
To use the chart module you call one of the chart methods on a collection object. Charts are an optional
module so make sure that your version of ForerunnerDB has the Highcharts module included.

#### collection.pieChart()

Function definition:

	collection.pieChart(selector, keyField, valField, seriesName);
	
Example:

	// Create the collection
	var coll = db.collection('chartData');
	
	// Set the collection data
	coll.setData([{
		name: 'Jam',
		val: 100
	}, {
		name: 'Pie',
		val: 33
	}, {
		name: 'Cake',
		val: 24
	}]);
	
	// Create a pie chart on the element with the id "demo-chart"
	coll.pieChart('#demo-chart', 'name', 'val', 'Food', {
		chartOptions: {
			title: {
				text: 'Food Eaten at Event'
			}
		}
	});

> Note that the options object passed as the 5th parameter in the call above has a chartOptions key. This
 key is passed to Highcharts directly so any options that are described in the Highcharts documentation
 should be added inside the chartOptions object. You'll notice that we set the chart title in the call
 above using this object.

#### collection.lineChart()

Function definition:

	collection.lineChart(selector, seriesField, keyField, valField);

Example:

	// Create the collection
	var coll = db.collection('chartData');

	// Set the collection data
	coll.setData([{
		type: 'Jam',
		date: String(new Date('2014-09-13')).substr(0, 15),
		val: 100
	}, {
		type: 'Jam',
		date: String(new Date('2014-09-14')).substr(0, 15),
		val: 33
	}, {
		type: 'Jam',
		date: String(new Date('2014-09-15')).substr(0, 15),
		val: 24
	}]);

	// Create a pie chart on the element with the id "demo-chart"
	coll.lineChart('#demo-chart', 'type', 'date', 'val', {
		chartOptions: {
			title: {
				text: 'Jam Stores Over Time'
			}
		}
	});

> Note that the options object passed as the 5th parameter in the call above has a
chartOptions key. This key is passed to Highcharts directly so any options that are
described in the Highcharts documentation should be added inside the chartOptions
object. You'll notice that we set the chart title in the call above using this object.

#### Other Chart Types

The lineChart() function uses the same parameters as the rest of the chart types currently supported by
ForerunnerDB:

* collection.barChart()
* collection.columnChart()
* collection.areaChart()

### Removing a Chart

You can drop a chart using the dropChart() method on the collection the chart is assigned to:

Function definition:

	collection.dropChart(selector);
	
Example:

	coll.dropChart('#demo-chart);
	
> Dropping a chart will remove it from the DOM and stop all further collection updates
from propagating to Highcharts.

# Special Considerations
## Queries
Queries are made up of properties in an object. ForerunnerDB handles some properties
differently from others. Specifically properties that start with a dollar symbol ($)
or two slashes (//) will be treated as special cases.

### The Dollar Symbol
Properties that start with a dollar symbol are treated as *operators*. These are not
handled in the same way as normal properties. Examples of operator properties are:

	$or
	$and
	$in

These operator properties allow you to indicate special operations to perform during
your query.

### The Double-Slash
> Version >= 1.3.14

Properties that start with a double-slash are treated as comments and ignored during
the query process. An example would be where you wish to store some data in the query
object but you do not want it to affect the outcome of the query.

	// Find documents that have a property "num" that equals 1:
	db.collection('test').find({
		'num': 1
	});
	
	// Find documents that have a property "num" that equals 1
	// -- this is exactly the same query as above because the //myData
	// property is ignored completely
	db.collection('test').find({
		'num': 1,
		'//myData': {
			'someProp': 134223
		}
	});

# Differences Between ForerunnerDB and MongoDB
Developers familiar with the MongoDB query language will find ForerunnerDB quite similar
however there are some fundamental differences that you should be aware of when writing
queries for ForerunnerDB.

> An update is being worked on that will allow a MongoDB emulation mode flag to be set
to force ForerunnerDB to behave exactly like MongoDB when running find and update
operations. For backward compatibility we cannot enable this by default or simply
change default operation of CRUD calls.

## find
ForerunnerDB uses objects instead of dot notation to match fields. See issue [#43](https://github.com/irrelon/ForerunnerDB/issues/43) for more
information. The reason we do this is for performance.

## update
ForerunnerDB runs an update rather than a replace against documents that match the query
clause. You can think about ForerunnerDB's update operations as having been automatically
wrapped in the MonogDB $set operator.

# Development

## Unit Tests
Unit tests are available in the ./unitTests folder, load index.html to run the tests.

## Building / Compiling
> This step is not required unless you are modifying ForerunnerDB code and wish to
build your own version.

ForerunnerDB uses Browserify to compile to single-file distribution builds whilst
maintaining source in distinct module files. To build, ensure you have Node.js and
browserify installed. To install browserify if you already have Node.js:

```
npm install -g browserify
```

Now you can then execute browserify to build ForerunnerDB:

```
browserify ./js/build/all.js -o ./js/dist/fdb-all.js
```

Grunt is used to execute regular tasks like build and test. Install grunt to access the
grunt tasks.

## Continuous Compiling
Browserify will compile to a single-file each time you run it. If you would prefer to
automatically compile each change (for faster development) you can run watchify instead.
Install watchify:

```
npm install -g watchify
```

You can then run watchify using the same command line arguments as browserify:

```
watchify ./js/build/all.js -o ./js/dist/fdb-all.js
```

The fully minified version of ForerunnerDB is run through uglifyjs as part of the grunt
build process.

## Contributing to This Project
Contributions through pull requests are welcome. Please ensure that if your pull request includes code changes that you have run the unit tests and they have all passed. If your code changes include new features not currently under test coverage from existing unit tests please create new unit tests to cover your changes and ensure they work as expected.

Code style is also important. Tabs are in use instead of spaces for indentation. Braces should start at the end of lines rather than the next line down. Doc comments are in JSDoc format and must be fully written for public-facing methods in any code you write. Private methods also require doc comments in master but can be merged into the dev branch without them.

So to summarise:

* Always check unit tests are running and passing
* Create new tests when you add or modify functionality that is not currently under test coverage
* Make sure you document your code with JSDoc comments

# iOS Version
You may notice in the repo that there is an iOS folder containing a version of Forerunner
for iOS. This project is still at an alpha level and should be considered non-production
code, however you are welcome to play around with it and get a feel for what will be
available soon.

The iOS version is part of the roadmap and will include data-binding for list structures
like UITableView, as well as individual controls like UILabel. Data-persistence is already
working as well as inserting and basic data queries, update and remove.

# Future Updates
ForerunnerDB's project road-map:

### Roadmap Completed Items
* Views that can join multiple documents together and data-bind - sort of like virtual collections
* Primary key violation checking
* Collection indexing
* Index violation checking
* Unit tests
* Server-side login and CRUD security - allow client login to server with pre-determined credentials that can be locked down to CRUD not only on particular collections but also only matching documents-- e.g. a user account could have a CRUD security record containing {profileId: '352349thj439yh43'} so that only documents that match that query can be edited by the user. This means they would only have update privilege on their own records as an example, but their read privilege could be {} allowing read on all documents.
* Query remote database from browser
* Data persistence on client-side
* NPM installation
* Rewritten data propagation system
* Added new class "Document" to allow data-binding against a single document object
* Added new class "Overview" to allow data-binding against auto-propagated map-reduce data
* Rewritten view sync system for performance

#### Supported query operators
* $ (array positional)
* $gt
* $gte
* $lt
* $lte
* $or
* $and
* $exists
* $push
* $addToSet
* $pull
* $in
* $nin
* $mul
* $rename
* $unset
* $pop
* $position
* $each
* $pullAll
* $elemMatch (projection)

#### Operators that are unique to ForerunnerDB:
* $move
* $splicePush
* $elemsMatch (projection)

### Future Updates
* Data persistence on server-side
* Collection / query paging-- e.g. select next 10, select previous 10
* Pull from server - allow client-side DB to auto-request server-side data especially useful when paging
* Push to clients - allow server-side to push changes to client-side data automatically and instantly
* Push to server - allow client-side DB changes to be pushed to the server automatically (obvious security / authentication requirements)
* Replication - allow server-side DB to replicate to other server-side DB instances on the same or different physical servers
* Native iOS version
* Native Android version

#### Query operators still to implement
* $setOnInsert
* $min
* $max
* $currentDate
* $slice
* $sort
* $bit
* $isolated
* $ array positional in sub arrays of objects inside arrays e.g. arr.$.idArr

#### Scheduled Features - Version 1.3
* Data-bound grid (table) output of collection / view data - COMPLETE
* $elemMatch (projection) - COMPLETE
* Return limited fields on query - COMPLETE

#### Scheduled Features - Version 1.4
* Fix package.json to allow dev dependencies and production ones, also fix versions etc (https://github.com/irrelon/ForerunnerDB/issues/6) - COMPLETE
* Data persistence added to documentation - COMPLETE
* Remove iOS from this repo, add to its own - COMPLETE
* Remove server from this repo, add to its own - COMPLETE
* Trigger support - COMPLETE
* Support localforage for storage instead of relying on localStorage (https://github.com/irrelon/ForerunnerDB/issues/5) - COMPLETE
* Highcharts support from views instead of only collections
* Fix bug in relation to index usage with range queries as per (https://github.com/irrelon/ForerunnerDB/issues/20)

#### Scheduled Features - Version 1.5
* Add further build files to handle different combinations of modules (https://github.com/irrelon/ForerunnerDB/issues/7)
* Add caching system so requests to a collection with the same query multiple times should generate once and serve the cached results next time round. Cache invalidation can be done on any CRUD op to make subsequent query re-build cache.

#### Scheduled Features - Version 1.6
* Support Angular.js by registering as a module if ajs exists (https://github.com/irrelon/ForerunnerDB/issues/4)
* Server-side operation in line with other production databases (e.g. command line argument support, persist to disk etc)

# Breaking Changes
Please check below for details of any changes that break previous operation or
behaviour of ForerunnerDB. Changes that break functionality are not taken lightly
and we do not allow them to be merged in to the master branch without good cause!

## Since Version 1.3.36
In order to support multiple named databases Forerunner's instantiation has changed
slightly. In previous versions you only had access to a single database that you
instantiated via:

	var db = new ForerunnerDB();

Now you have access to multiple databases via from the main forerunner instance but this
requires that you change your instantiation code to:

	var fdb = new ForerunnerDB();
	var db = fdb.db('myDatabaseName');

Multiple database support is a key requirement that unfortunately requires we change
the instantiation pattern as detailed above. Although this is a fundamental change to
the way ForerunnerDB is instantiated we believe the impact to your projects will be
minimal as it should only require you to update at most 2 lines of your project's code
in order to "get it working" again.

To discuss this change please see the related issue: [https://github.com/Irrelon/ForerunnerDB/issues/44](https://github.com/Irrelon/ForerunnerDB/issues/44)

## Since Version 1.3.10
The join system has been updated to use "$join" as the key defining a join instead of
"join". This was done to keep joins in line with the rest of the API that now uses
the $ symbol when denoting an operation rather than a property. See the Joins section
of the documentation for examples of correct usage.

Migrating old code should be as simple as searching for instances of "join" and
replacing with "$join" within ForerunnerDB queries in your application. Be careful not
to search / replace your entire codebase for "join" to "$join" as this may break other
code in your project. Ensure that changes are limited to ForerunnerDB query sections.
