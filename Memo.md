1. **Users Table:**
    - user_id (Primary Key)
        - username
        - password (hashed)
        - full_name
        - role (Cashier, Manager, Admin, etc
        - sales_made
        
    
2. **Customers Table:**
    - customer_id (Primary Key)
        - first_name
        - last_name
        - email
        - phone
        - address
        - loyalty_points
        - total_purchases
        
    
3. **Products Table:**
    - product_id (Primary Key)
        - name
        - description
        - category
        - price
        - stock_quantity
        - supplier_id (Foreign Key to Suppliers)
        - total_sales
        
    
4. **Suppliers Table:**
    - supplier_id (Primary Key)
    - name
     - contact_name
    - email
    - phone
     - address
        
    
5. **Sales Table:**
    - sale_id (Primary Key)
    - cashier_id (Foreign Key to Users)
    - customer_id (Foreign Key to Customers)- sale_date
    - total_amount
    - payment_method (Cash, Credit Card, etc.)
    - profit
   - cost_price
  - items_sold
        
    
6. **SaleItems Table:**
    - sale_item_id (Primary Key)
    - sale_id (Foreign Key to Sales)
    - product_id (Foreign Key to Products)
    - quantity_sold
     - unit_price
    - subtotal
        
    
7. **Payments Table:**
    - payment_id (Primary Key)
     - sale_id (Foreign Key to Sales)
    - payment_date
    - amount
    - payment_method (Cash, Credit Card, etc.)
        
    
8. **TransactionLogs Table:**
    - log_id (Primary Key)
    - user_id (Foreign Key to Users)
    - action (e.g., Sale, Return, Product Update)
    - timestamp
    - description
        
    
9. **Store Table**
    - store_id (Primary Key)
    - rent
---
PoS의 DB 스키마를 이렇게 짰는데 어떤 기능을 만들면 될까? 