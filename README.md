## MongoDB Documentation

### PART 1

#### InsertOne

	db.getCollection('demo').insertOne(
		{
			"sports":"cricket",
			"type":"outdoor",
			"players":11,
			"stadium":"chepauk"
		}
	)

#### InsertMany

	db.getCollection('demo').insertMany(
		[
			{
				"sports":"football",
				"type":"outdoor",
				"players":11,
				"stadium":"campnou"
			},
			{
				"sports":"basketball",
				"type":"indoor",
				"players":5,
				"stadium":"staples centre"
			},
			{
				"sports":"volleyball",
				"type":"indoor",
				"players":6,
				"stadium":"long beach arena"
			}
		]
	)

#### Find

	db.getCollection('demo').find({})
	db.getCollection('demo').find({
        $or: [
            {'id': 1.0},
            {'id': 2.0}
        ]
	})
	db.getCollection('demo').find({
		$and: [
			{ $or: [ { qty: { $lt : 10 } }, { qty : { $gt: 50 } } ] },
			{ $or: [ { sale: true }, { price : { $lt : 5 } } ] }
		]
	})

	db.getCollection('demo').find({
		$or: [
			{
				$and: [
					{ signup_date: { $ne: null } },
					{ last_visited: { $gte: ISODate("2021-08-18T00:13:33.532Z") } }
				]
			},
			{
				$or: [
					{ chat_session_count: { $gt: 0 } }, 
					{ formdata_count: { $gt: 0 } }
				]
			}
		]
	}).pretty()

#### FindOne

	db.getCollection('demo').findOne(
		{
			'name':'your name'
		}
	)

	db.getCollection('users').find(
		{
			"useremail": {$regex: /^testing/, $options: 'i'}
		}
	)

#### UpdateOne

	db.getCollection('demo').update(
		{
			_id:ObjectId("5dccf2fbb6d98b3375014ed5")
		},
		{
			$set: {
				"stadium":"eden garden"
			}
		}
	)

#### UpdateMany

	db.getCollection('demo').updateMany(
		{},
		{
			$set:{
				"fieldname":"value"
			}
		}
	)

	db.getCollection('posts').update(
		{ _id: { $in: [ObjectId("60fd14cb5f67a5537f1cd5b3"),ObjectId("60fe96ef5f67a5537f1cd5b4")] } },
		{ $set: { publish : false } },
		{ multi: true }
	)

	db.posts.find({}).forEach(function(e,i) {
    	e.feature_image = e.feature_image.replace("https://blogimproductive.s3.amazonaws.com/blogs","https://improductive.com/blog");
    	db.posts.save(e);
	});

#### Deleteone

	db.getCollection('demo').deleteOne(
		{
			"_id" : ObjectId("5dccf2fbb6d98b3375014ed5")
		}
	);

#### DeleteMany

	db.getCollection('videos').deleteMany({
		$and: [
			{'createdby':ObjectId('5fc9c7984ca9074c54a9ded4')},
			{'status': 'created'}
		]
  	})
---
	db.getCollection('entities').remove({ 
		_id : { 
			$in: [
				ObjectId("5e186e939c3ddd5e5379d12e"), 
				ObjectId("5e186f2e0353105f10afc965")
			]
		}
	});

---


### PART 2

#### Sort

	Ascending 

		db.getCollection('demo').find({}).sort({KEY:1})
		
	Descending 

		db.getCollection('demo').find({}).sort({KEY:-1})

#### limit

	db.getCollection('demo').find({}).limit(2)

#### date

	db.getCollection('demo').updateMany(
		{},{
			$set: {
				"dateAdded":new Date()
			}
		}
	)

#### pagination

	db.getCollection('demo').find({}).skip(2)

---

### PART 3

#### update in sub document

	db.getCollection('demo').updateOne(
		{
			_id:ObjectId("5dccf8fdb6d98b3375014ed9")
		},
		{
			$set:{
				"names":["xxx","yyy","zzz"]
			}
		}
	)


#### equals

	db.getCollection('demo').find(
		{
			players:{
				$eq:11
			}
		}
	)

#### not equals

	db.getCollection('demo').find(
		{
			type:{
				$ne:"outdoor"
			}
		}
	)

#### count

	db.getCollection('demo').count()

### Aggregate

#### Match

	db.getCollection('demo').aggregate(
		[
			{
				$match:{
					type:"indoor"
				}
			}
		]
	)

#### Group

	db.getCollection('demo').aggregate(
		[
			{
				$group:{
					_id:"$type"
				}
			}
		]
	)


#### Sum

	db.getCollection('students').aggregate(
		[
			{
				$group:{
					_id: "$name",
					"Total":{
						$sum: "$marks"
					}
				}
			}
		]
	)


#### Lookup

	db.getCollection('students').aggregate(
		[
			{
				$project: {
					"_id": {
						"$toString": "$_id"
					}
				}
			},
			{
				$lookup: {
					"from": "employees",
					"localField": "_id",
					"foreignField": "userid",
					"as": "user_data"
				}
			}
		]
	)

#### Project

	db.getCollection('demo').aggregate(
		[
			{
				$project:{
					sports:1,
					type:1
				}
			}
		]
	)

#### Exists

	db.reports.find( { predicted: { $exists: true } } )
---


### PART 4

#### Insert Document

<!-- if valid json file use --jsonArray -->
	
	mongoimport --db practice --collection restaurants --file restaurants.json
