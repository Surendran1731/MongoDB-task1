 # MongoDB Queries

This repository contains MongoDB queries for Day-1 task.

## Day 1 Queries

1. **Find all the information about each product**
    ```javascript
    db.products.find().pretty()
    ```

2. **Find the product price which is between 400 to 800**
    ```javascript
    db.products.find({product_price:{$gt:400,$lt:800}}).pretty()
    ```

3. **Find the product price which is not between 400 to 600**
    ```javascript
    db.products.find({product_price:{$not:{$gte:400,$lte:600}}}).pretty()
    ```

4. **List the four products which are greater than 500 in price**
    ```javascript
    db.products.find({product_price:{$gt:500}}).limit(4).pretty()
    ```

5. **Find the product name and product material of each product**
    ```javascript
    db.products.find({},{product_name:1,product_material:1}).pretty()
    ```

6. **Find the product with a row id of 10**
    ```javascript
    db.products.find({id:{$eq:"10"}}).pretty()
    ```

7. **Find only the product name and product material**
    ```javascript
    db.products.find({},{product_name:1,product_material:1,id:0}).pretty()
    ```

8. **Find all products which contain the value 'Soft' in product material**
    ```javascript
    db.products.find({product_material: 'Soft'}).pretty()
    ```

9. **Find products which contain product color 'indigo' and product price 492.00**
    ```javascript
    db.products.find({$or:[{product_color:'indigo'},{product_price:'492.00'}]}).pretty()
    ```

10. **Delete the products which have the same product price value**
    ```javascript
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
    ```
