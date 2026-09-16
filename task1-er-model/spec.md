Сутності та їх атрибути: 

CUSTOMER (id, name, email, phone, address)
PRODUCT (id, name, description, price, stock_quantity)
ORDER (id, customer_id (FK), order_date, status, shipping)

erDiagram
    
    CUSTOMER {
        id
        name
        email
        phone
        address
    }

    ORDER {
        id
        customer_id (FK)
        order_date
        status
        shipping
    }

    PRODUCT {
        id
        name
        description
        price
        stock_quantity
    }
