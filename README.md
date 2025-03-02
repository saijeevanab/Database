# Database Design Document

## Entity-Relationship Diagram (ERD)

### Entities and Relationships

The database consists of the following entities:

1. **User**: Stores user information.
2. **Product**: Stores product details.
3. **Order**: Represents user orders.
4. **Order_Item**: Stores items in each order.
5. **Payment**: Manages payment details.

### ERD Diagram

<img width="468" alt="image" src="https://github.com/user-attachments/assets/197295b0-2533-451b-94ca-b44221c12d0a" />



---

## Database Tables

### User Table

```
CREATE TABLE User (
    user_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
### Product Table

```
CREATE TABLE Product (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    stock INT NOT NULL,
    quantity INT NOT NULL,
    category VARCHAR(50) NOT NULL
);
```
### Order Table

```
CREATE TABLE `Order` (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (user_id) REFERENCES User(user_id) ON DELETE CASCADE
);
```
### Order_Item Table

```
CREATE TABLE Order_Item (
    order_item_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES `Order`(order_id) ON DELETE CASCADE,
    FOREIGN KEY (product_id) REFERENCES Product(product_id) ON DELETE CASCADE
);
```
### Payment_Table
```
CREATE TABLE Payment (
    payment_id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT UNIQUE NOT NULL,
    amount DECIMAL(10,2) NOT NULL,
    payment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    payment_status ENUM('Pending', 'Completed', 'Failed') NOT NULL,
    FOREIGN KEY (order_id) REFERENCES `Order`(order_id) ON DELETE CASCADE
);
```
## Sample SQL Queries
###  1. Insert Sample Data

```
INSERT INTO User (name, email, password)
VALUES
    ('Sai Jeevana', 'jeevanabanothu@gmail.com', '12345'),
    ('Sai Pavan', 'saipavan@gmail.com', '67890');
```
## 2. Insert an Order
```
INSERT INTO `Order` (user_id, total_amount)
VALUES (1, 1699.49);
```
## 3. Insert Order Items
```
INSERT INTO Order_Item (order_id, product_id, quantity, price)
VALUES (1, 1, 1, 999.99), (1, 2, 1, 699.50);
```
## 4. Insert a Payment Record
```
INSERT INTO Payment (order_id, amount, payment_status)
VALUES (1, 1699.49, 'Completed');
```
## 5. Retrieve All Orders with User Info
```
SELECT o.order_id, u.name, u.email, o.order_date, o.total_amount, p.payment_status
FROM `Order` o
JOIN User u ON o.user_id = u.user_id
LEFT JOIN Payment p ON o.order_id = p.order_id;
```
