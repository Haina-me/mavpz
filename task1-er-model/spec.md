Сутності та їх атрибути: 

CUSTOMER (id, name, email, phone)
PRODUCT (id, name, description, price, stock_quantity)
ORDER (id, customer_id (FK), int payment_data_id (FK), shipping_address_id (FK), order_date, status)
ADDRESS (id, customer_id (FK), street, city, postal_code, country, shipping)
PAYMENT_DATA (id (PK), customer_id (FK), card_type, card_namber, card_CVV)
ORDER_PRODUCT (id, order_id (FK), product_id (FK), quantity, fix_price)


erDiagram

    CUSTOMER ||--o{ ORDER : ""
    CUSTOMER ||--o{ ADDRESS : ""
    CUSTOMER ||--o{ PAYMENT_DATA : ""
    ORDER }o--|| ADDRESS : ""
    ORDER }o--|| PAYMENT_DATA : ""
    ORDER ||--o{ ORDER_PRODUCT : ""
    PRODUCT ||--o{ ORDER_PRODUCT : ""

    CUSTOMER {
        int id PK
        string name
        string email
        string phone
    }

    ORDER {
        int id PK
        int customer_id FK
        int payment_data_id FK
        int shipping_address_id FK
        date order_date
        string status
    }

    PRODUCT {
        int id PK
        string name
        string description
        decimal price
        int stock_quantity
    }

    ADDRESS {
        int id PK
        int customer_id FK
        string street
        string city 
        string postal_code 
        string country 
        string shipping
    }

    PAYMENT_DATA {
        int id PK
        int customer_id FK
        string card_type 
        string card_number 
        string card_CVV
    }
        
    ORDER_PRODUCT {
        int id PK
        int order_id FK
        int product_id FK
        int quantity
        decimal fix_price
    }
