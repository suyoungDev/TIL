## 📢 schema(개요) 작성

```
	CREATE TABLE products (
	  	id INT NOT NULL,    
	  	name STRING,
		  price MONEY,
	  	PRIMARY KEY (id)
	  )
```

- NOT NULL 👉 필수 작성
- SQL Data Types: string, money
- primary key는 고유한 값으로 데이터식별하는 키. 보통 id로 쓰임



### 작성 규칙:
- 데이터 테이블 만듦: 테이블이름 products ( 개요 )
- 형식: 데이터이름 / 데이터 타입 / 콤마
- () 중괄호안에 ; 없이 , 콤마로만 작성.
	



## ⚡ 데이터베이스 작성

### 작성 방법:
```
INSERT INTO table_name (column1, column2, …)
VALUES (value1, value2, …)
```
	
**예시1**: (모든 column에 값 넣을 경우):
```
INSERT INTO products
VALUES (1, 'pen', 1.5)
```
	
**예시2** :
```
INSERT INTO products(id, name)
VALUES(2, 'pencil')
```

## ⚡ 데이터 추가
**예시**
```
SELECT * 
FROM Customers
Where Country='Mexico';
```
👉 country가 'Mexico'인 손님테이블의 모든걸 보여줘
	
**예시**
```
SELECT id, name, tear  // 이렇게 특정요소만 확인가능
FROM hangAhRi 
```

**예시**

```
SELECT id, name, tear 
FROM hangAhRi
WHERE id = 3
```

## ⚡ 데이터 수정
**예시**
```
	UPDATE products
	SET price = 0.80
	WHERE id = 2
```

👉 prodcuts 테이블의 ID가 2인 것의 price를 0.80으로 update해줘
	

## ⚡ 데이터 추가하기

**➕alter**:
add, delete, modify columns in an existing table
	
	
**예시**
```
ALTER TABLE products
ADD stock INT
```

## ⚡ 데이터 삭제하기
**예시**
```
DELETE FROM table_name
WHERE condition;
```

✔where 없으면 모든게 지워지므로 항상 where condition을 적었는지 확인할 것!

## foreign Key
> foreign key는 다른 두 테이블을 서로 연결하는데 사용된다.
> 외부 키는 다른 테이블의 기본 키를 참조하는 한 테이블의 필드(또는 필드 모음)임.

**예시**
```
		CREATE TABLE orders (
		  	id INT NOT NULL,
			   order_number INT,
		  	customer_id int,
		  	product_id INT,
		 	PRIMARY KEY (id),
		  	FOREIGN KEY (customer_id) REFERENCES customers (id),
		  	FOREIGN KEY (product_id) REFERENCES products(id),
		  )
```	
👉 customer_id는 customers 테이블의 id를 참조하는 foreign key
👉 product_id는 products 테이블의 id를 참조하는 foreign key


## ⚡ JOIN
테이블을 모두 합치는 기능
inner Join이 자주 쓰임(그외에 많음)
	
**예시**
```
	SELECT Orders.OrderID, Customers.CustomerName
	FROM Orders
	INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```
**예시**
```
	SELECT orders.order_number, products.name, products.price, products.stock
	FROM orders
	INNER JOIN products on orders.product_id = products.id
```
	
✔즉,
		SELECT 원하는 columns
		FROM 참조 테이블1
		INNER JOIN 참조 테이블2 ON 조건

