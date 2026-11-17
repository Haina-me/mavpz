# Генерація Sequence-діаграм для US-2-3 (Оформлення замовлення)

**Мій запит:**
> Ти System Architect та експерт з Mermaid. Твоє завдання — згенерувати Sequence-діаграми для 4х Gherkin-сценаріїв (Use Case: Оформлення замовлення).
> ПРАВИЛА РОБОТИ (КРИТИЧНО ВАЖЛИВО):
>  Учасники повинні строго відповідати сутностям ER-моделі (наприклад, Customer, Address, PaymentMethod, Cart, Orders, Payment). Можеш додати Frontend (Браузер), Backend (Сервер) та зовнішній Bank (Платіжний шлюз). Жодних вигаданих таблиць.
> Методи: Використовуй реалістичні назви функцій ( getAddresses(), validatePayment(), clearCart()).
Фрагменти: Мінімум один комбінований фрагмент (alt, opt або loop) у КОЖНІЙ діаграмі.
> Activation bars (ЖОРСТКА ВИМОГА): Лінія життя (activate / deactivate) учасника, що працює поза фрагментом, має починатися ДО фрагмента і закінчуватися ПІСЛЯ нього. Жодних обривів, дублювань чи deactivate всередині окремих  гілок alt чи opt

**Результат від AI:**

**Для КП1:**
```mermaid
sequenceDiagram
    autonumber
    actor User as Покупець
    participant Frontend
    participant Backend
    participant Cart as БД: Cart
    participant Address as БД: Address
    participant PaymentMethod as БД: PaymentMethod

    User ->> Frontend: Натискає "Оформити замовлення"
    activate Frontend
    
    Frontend ->> Backend: getCheckoutData(customer_id)
    activate Backend
    
    Backend ->> Cart: getCartItems(customer_id)
    activate Cart
    Cart -->> Backend: items (List<CartItem>)
    deactivate Cart
    
    alt cart has items
        Backend ->> Address: getActiveAddresses(customer_id)
        activate Address
        Address -->> Backend: addressList
        deactivate Address
        
        Backend ->> PaymentMethod: getActivePaymentMethods(customer_id)
        activate PaymentMethod
        PaymentMethod -->> Backend: paymentList
        deactivate PaymentMethod
        
        Backend -->> Frontend: 200 OK (items, addressList, paymentList)
    else cart is empty
        Backend -->> Frontend: 400 Bad Request (Cart is empty)
    end
    deactivate Backend

    Frontend -->> User: Відкриває сторінку оформлення

    opt User selects existing data
        User ->> Frontend: Обирає збережену адресу
        User ->> Frontend: Обирає збережений спосіб оплати
    end
    deactivate Frontend
```
**Для КП2:**
```mermaid
sequenceDiagram
    autonumber
    actor User as Покупець
    participant Frontend
    participant Backend

    User ->> Frontend: Натискає "Підтвердити замовлення" (дані не обрано)
    activate Frontend
    
    Frontend ->> Backend: createOrder(customer_id, address_id=null, payment_id=null)
    activate Backend
    
    alt is missing address or payment method
        Backend -->> Frontend: 400 Bad Request (Validation Error)
    else parameters are valid
        Note over Backend: Продовження процесу (не в цьому сценарії)
    end
    
    deactivate Backend

    Frontend -->> User: Показує повідомлення про необхідність обрати дані
    
    deactivate Frontend
```
**Для КП3:**

```mermaid
sequenceDiagram
    autonumber
    actor User as Покупець
    participant Frontend
    participant Backend
    participant Bank as Bank (Шлюз)
    participant Orders as БД: Orders
    participant Payment as БД: Payment
    participant Cart as БД: Cart

    User ->> Frontend: Підтверджує замовлення (обрано адресу та оплату)
    activate Frontend
    
    Frontend ->> Backend: checkout(customer_id, address_id, payment_method_id)
    activate Backend
    
    Backend ->> Bank: processPayment(amount, bank_token)
    activate Bank
    Bank -->> Backend: 200 OK (transaction_id)
    deactivate Bank
    
    alt payment is successful
        Backend ->> Orders: insertOrder(customer_id, address_id, total_amount)
        activate Orders
        Orders -->> Backend: order_id
        deactivate Orders
        
        Backend ->> Payment: recordPayment(order_id, payment_method_id, amount, status="success")
        activate Payment
        Payment -->> Backend: payment_id
        deactivate Payment
        
        Backend ->> Cart: clearCart(customer_id)
        activate Cart
        Cart -->> Backend: success (кошик очищено)
        deactivate Cart
        
        Backend -->> Frontend: 200 OK (Order Created)
        Frontend -->> User: Показує сторінку успішного оформлення
    else payment is failed
        Backend -->> Frontend: 402 Payment Required
        Frontend -->> User: Показує повідомлення про відмову в оплаті
    end
    
    deactivate Backend
    deactivate Frontend
```
