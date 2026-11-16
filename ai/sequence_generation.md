# Генерація Sequence-діаграм для US-2-3 (Оформлення замовлення)

## Сценарій 1: Відкриття сторінки оформлення та вибір даних

**Мій запит:**
> Ти System Architect та експерт з Mermaid. Твоє завдання — згенерувати Sequence-діаграми для 4х Gherkin-сценаріїв (Use Case: Оформлення замовлення).
> ПРАВИЛА РОБОТИ (КРИТИЧНО ВАЖЛИВО):
>ПО ЧЕРЗІ: Ми працюємо ітеративно. Ти генеруєш код Mermaid ТІЛЬКИ для першого сценарію і ЗУПИНЯЄШСЯ. Чекаєш на мої правки або затвердження. Тільки після мого повідомлення "Затверджено, роби наступний" ти переходиш до другого сценарію, і так далі.
> Аудит учасників: Учасники (БД/Сервіси) повинні строго відповідати сутностям ER-моделі (наприклад, Customer, Address, PaymentMethod, Cart, Orders, Payment). Можеш додати Frontend (Браузер), Backend (Сервер) та зовнішній Bank (Платіжний шлюз). Жодних вигаданих таблиць.
> Методи: Використовуй реалістичні назви функцій ( getAddresses(), validatePayment(), clearCart()).
Фрагменти: Мінімум один комбінований фрагмент (alt, opt або loop) у КОЖНІЙ діаграмі.
> Activation bars (ЖОРСТКА ВИМОГА): Лінія життя (activate / deactivate) учасника, що працює поза фрагментом, має починатися ДО фрагмента і закінчуватися ПІСЛЯ нього. Жодних обривів, дублювань чи deactivate всередині окремих  гілок alt чи opt! 

**Результат від AI:**
```mermaid
[sequenceDiagram
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
    deactivate Frontend]