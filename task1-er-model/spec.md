Сутності та їх атрибути: 

CUSTOMER (id, name, email, phone, address)
PRODUCT (id, name, description, price, stock_quantity)
ORDER (id, customer_id (FK), order_date, status, shipping)
ADDRESS (id, customer_id (FK), street, city, postal_code, country, type shipping)
PAYMENT (id (PK), customer_id (FK), card details)
ORDER_PRODUCT (id, order_id (FK), product_id (FK), quantity)


erDiagram
    
    CUSTOMER {
        id (PK)
        name
        email
        phone
    }

    ORDER {
        id PK
        customer_id (FK)
        order_date
        status
        shipping
        shipping_address_id (FK)
    }

    PRODUCT {
        id (PK)
        name
        description
        price
        stock_quantity
    }

    ADDRESS {
        id (PK)
        customer_id (FK) 
        street
        city 
        postal_code 
        country 
        type shipping
    }

    PAYMENT {
        id (PK) 
        customer_id (FK) 
        card details
    }
        
    ORDER_PRODUCT {
        id, order_id (FK) 
        product_id (FK)
        quantity
    }
