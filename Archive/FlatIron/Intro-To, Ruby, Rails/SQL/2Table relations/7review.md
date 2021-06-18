Ecomerce Table

```
products
1iphone
2radio
3tv

line_items
cart_id   product_id
   1		1
carts
id		user_id		order_dat
1			1		3/02/16
2			1		3/02/16

customer
1 avi
```

 

```sqlite
--what is the total price of cart?
--I need to find the car by id
--find all lin Items with that cart id
--then fimnd all the product_id in those lime items rows
--then lookup products.name for the product_id from those line_item rows
SELECT products.* FROM products
LEFT JOIN line_items ON line_items.product_id = products.id
WHERE line_items.cart_id = 1

--#what products are in a cart?
SELECT sum(products.price) AS total_price FROM products
JOIN line_items ON line_items.poduct_id = products.id
WHERE line_items.cart_id = 1
--#what's valuable customer?

--#what is the most popular product?
```

