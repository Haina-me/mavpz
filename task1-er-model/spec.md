Сутності та їх атрибути: 

CUSTOMER (name, e-mail, phone, address)
PRODUCT (description, quantity, productprice)
ORDER (orderid, status, orderprice)

erDiagram
    
    CUSTOMER {
        string custid
        string name
        string email
        string phone
        string address
    }

    ORDER {
        string orderid
        string status
        string orderprice
    }

    PRODUCT {
        string description
        string quantity
        string productprice
    }

    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ PRODUCT : places