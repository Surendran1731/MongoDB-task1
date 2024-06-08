# MongoDB-task1
#Day 1 Queries
Day 1 Queries
Find all the information about each product

db.products.find().pretty()
Find the product price which is between 400 to 800

db.products.find({product_price:{$gt:400,$lt:800}}).pretty()
Find the product price which is not between 400 to 600

db.products.find({product_price:{$not:{$gte:400,$lte:600}}}).pretty()
List the four products which are greater than 500 in price

db.products.find({product_price:{$gt:500}}).limit(4).pretty()
Find the product name and product material of each product

db.products.find({},{product_name:1,product_material:1}).pretty()
Find the product with a row id of 10

db.products.find({id:{$eq:"10"}}).pretty()
Find only the product name and product material

db.products.find({},{product_name:1,product_material:1,id:0}).pretty()
Find all products which contain the value 'Soft' in product material

db.products.find({product_material: 'Soft'}).pretty()
Find products which contain product color 'indigo' and product price 492.00

db.products.find({$or:[{product_color:'indigo'},{product_price:'492.00'}]}).pretty()
Delete the products which have the same product price value

db.products.aggregate([
    {
        $group: {
            _id: "$product_price",
            count: { $sum: 1 },
            ids: { $push: "$_id" }
        }
    },
    {
        $match: {
            count: { $gt: 1 }
        }
    }
]).forEach(function(doc) {
    doc.ids.shift();
    db.products.deleteMany({_id: {$in: doc.ids}});
});
print("Documents with duplicate product prices have been deleted successfully.");
