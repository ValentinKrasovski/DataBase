# DataBase
# 1. 
     * Красовский Валентин Алексеевич;
     * 153501;
     * кондитерская(магазин шоколада)
     
# 2. Минимальные функциональные требования:
    * User authorisation
    * User managment
    * Role system(user, admin)
    * Logging user action
    * For unauthorized user: watching branded cholates
    * For authorized user: make an order
    * For authorized user: see the detailes of chocolate(company, description, recepie)
    * For authorized user: link the card
    * For admin: stock managment
    * For admin: delivery managment
    * For admin: order managment
    * For admin: provider managment
    * For admin: recepie managment
    
# 3. Основные Сущности

1. ***Пользователь (Users)*** - представляет собой основную сущность, хранящую информацию о зарегистрированных пользователях.
    - user_id(PK): Уникальный идентификатор пользователя.
    - user_fio: Информация о имени, фамилии пользователя.
    - user_login: Уникальный логин пользователя, используется для аутентификации.
    - user_password: Хешированный пароль пользователя для безопасной аутентификации.
    - user_email: Уникальный адрес электронной почты пользователя.
    - adress_id: хранит информацию о стране и городе проживания пользователя
    - role_id(FK): Многие пользователи могут иметь множество ролей через сущность "Роль пользователя". 
    - card_id(FK): Пользователь может привязать банковскую карту для оплаты покупок онлайн. 
       
2. ***Роль (Roles)***
    - role_id(PK): Уникальный идентификатор роли.
    - role_name: Название роли, например, "администратор", "пользователь", "Поставщик".
      
3. ***Склад(Stock)*** - хранит информацию о товарах и их количестве, хранящихся на складе
   - stock_id(PK) - уникальный идентификатор склада
   - choco_id(FK) - идентификатор продукта
   - choco_count - количество товара
      
4. ***Chocolate*** - хранит информацию о готовом продукте.
    - choco_id(PK): Уникальный идентификатор продукта.
    - choco_name: имя продукта
    - amount - количество шоколадок на складе 
    - choco_cost - Цена 1 шоколадки
    - discription - описание шоколадки
    - recepie_id(FK): рецепт шоколадки
      
5. ***Recepie*** - содержит информацию о рецепте шоколада.
    - recepie_id(PK): уникальный идентефикатор рецепта
    - chocolate_type_id(FK): идентификатор типа шоколада
    - filling_id(FK): идентификатор начинки
      
6. ***Chocolate_type*** - содержит информацию о типе шоколада(молочный, темный, белый)
    - chocolate_type_id(PK) - уникальный идентификатор типа шоколада
    - taste_name - имя вкуса(молочный, темный, белый)
    - gramms - количество шоколада 
  
7. ***Fillings_type*** - содержит информацию о начинке шоколадки
    - filling_id(PK) - уникальный идентификатор начинки
    - filling_name - имя начинки
    - grams - количество начинки 
              
8. ***Order*** - Эта сущность хранит информацию о заказе пользователя (промежуточная стадия, включающая статус заказа(оплачен/не оплачен), дату и время оформления)
    - order_id(PK): уникальный номер заказа
    - user_id(FK) - пользователь, сделавший заказ
    - order_datetime - дата и время оформления заказа
    - order_payment - статус оплаты заказа(оплачен/не оплачен)
    - order_cost - суммарная стоимость заказа на момент его оформления
    - deliv_id(FK) - идентификатор доставки заказа
        
9. ***Cards*** - Эта сущность хранит информацию о карте, привязанной к пользователю.
    - card_id(PK): Уникальный идентификатор карты
    - card_num - уникальный номер карты
    - card_date - время действия карты (до какого числа)

10. ***Delivery*** - хранятся данные о процессе доставки товара 
    - deliv_id(PK): Уникальный идентификатор
    - deliv_status: Статус заказа 
      
11. ***Provider*** - хранит информацию о производителе товара.
    - provider_id(PK): уникальный id производителя
    - prov_name: имя производителя
    - prov_discription: описание компании производителя
    - prov_contacts: контакты производителя
    - choco_id(FK): производимые товары
      
# 4 Ограничения

1. **Users**:
   - *user_id* INT PRIMARY KEY AUTO_INCREMENT
   - *user_fio* VARCHAR(60) NOT NULL
   - *user_login* VARCHAR(30) NOT NULL UNIQUE
   - *user_password* VARCHAR(10) NOT NULL UNIQUE
   - *user_email* VARCHAR(30) NOT NULL UNIQUE
   - *role_id* INT FOREIGN KEY REFERENCES Roles (role_id)
   - *card_id* INT FOREIGN KEY REFERENCES Cards(card_id)
     
2. **Roles**:
   - *role_id* INT PRIMARY KEY AUTO_INCREMENT
   - *role_name* VARCHAR(30) NOT NULL
     
3. **Stock**:
   - *stock_id* INT PRIMARY KEY AUTO_INCREMENT
   - *product_id* INT FOREIGN KEY Product(product_id)
   - *product_count* INT
     
4. **Chocolate**:
   - *choco_id* INT PRIMARY KEY AUTO_INCREMENT
   - *choco_name* VARCHAR(30) NOT NULL
   - *amount* INT
   - *choco_cost* DOUBLE NOT NULL
   - *discription* VARCHAR(200)
   - *recepie_id*(FK)
   
5. **Recepie**:
    - *recepie_id* INT PRIMARY KEY AUTO_INCREMENT
    - *chocolate_type_id*(FK)
    - *filling_id*(FK)

6. **Chocolate_type**:
    - *chocolate_type_id* INT PRIMARY KEY AUTO_INCREMENT
    - *taste_name* VARCHAR(30) NOT NULL
    - *grams* INT NOT NULL
    
7. **Fillings_type**:
    - *filling_id* INT PRIMARY KEY AUTO_INCREMENT
    - *filling_name* VARCHAR(30) 
    - *grams* INT 
     
8. **Order**:
   - *order_id* INT PRIMARY KEY AUTO_INCREMENT
   - *user_id* INT FOREIGN KEY Users (user_id)
   - *order_datetime* DATETAME 
   - *order_payment* BOOLEAN
   - *order_cost* DOUBLE NOT NULL
   - *deliv_id* INT FOREIGN KEY Delivery(deliv_id)
     
9. **Cards**:
   - *card_id* INT PRIMARY KEY AUTO_INCREMENT
   - *card_num* VARCHAR(16) NOT NULL UNIQUE
   - *card_date* DATE
     
10. **Delivery**:
   - *deliv_id* INT PRIMARY KEY AUTO_INCREMENT
   - *deliv_status* VARCHAR(40)
  
11. **Table Provider**:
   - *provider_id* INT PRIMARY KEY AUTO_INCREMENT
   - *prov_name* VARCHAR(30)
   - *prov_discription* VARCHAR(200) NOT NULL
   - *prov_contacts* VARCHAR(30)
   - *choco_id* INT FOREIGN KEY Product(product_id)
# 5 Диаграмма
![image](https://github.com/ValentinKrasovski/DataBase/assets/92800455/2614c098-5221-44de-86ac-d4ed2b997f53)


