# Ecommerce-Data-Cleaning-and-EDA-in-Pandas
У цьому файлі представлено покроковий опис виконання проєкту з дата клінінгу та EDA у Pandas
## 1. Завантаження [датасету із Kaggle](https://www.kaggle.com/datasets/nynkeugerard/dirty-ecommerce-data-eda-r)
В цьому датасеті багато даних, які навмисно були згенеровані з великою кількістю помилок для того, щоб попрактикуватись в очищенні даних
## 2. Імпорт необхідних бібліотек та датасету

![image](https://github.com/user-attachments/assets/e6c25343-6edf-4c31-956d-d7565ec172e5)

Після імпорту всього необхідного бачимо таблицю

![image](https://github.com/user-attachments/assets/12b6fad6-e6c4-4c07-8563-c7f3b5b8e3d9)

Також одразу можна переглянути коротку інформацію про цю таблицю

![image](https://github.com/user-attachments/assets/fec5e598-0c1c-4726-948a-c34187b13431)

Одразу кидається в очі, що є стовпці, які містять тип даних float, хоча мали б бути типу int, оскільки є цілими числами. Це виправимо у наступних кроках
## 3. Очищення даних
### 1) Видалення дублікатів
Видаляємо усі ідентичні рядки

![image](https://github.com/user-attachments/assets/457dd32d-ef69-44cc-bfb7-902345025dbf)

Після видалення дублікатів бачимо, що кількість записів зменшилась на 500 

![image](https://github.com/user-attachments/assets/7c0de272-3f01-4042-b3c4-e9606a76f070)

Оскільки існували повністю ідентичні рядки, отже попадались і повтори серед OrderID, які мали би бути унікальними значеннями. 

### 2) Унікальність OrderID
Перевіримо кількість унікальних OrderID 

![image](https://github.com/user-attachments/assets/69a08dec-aac6-48f6-a85e-dfe8f66e88b5)

Всього є 9500 замовлень, а унікальних OrderID лише 6123, що порушує умову унікальності. Тому необхідно створити новий стовпець OrderID, де всі значення будуть унікальними

![image](https://github.com/user-attachments/assets/798996b1-d53a-46f8-818a-c24e08a1b4cc)

Було створено додатковий DataFrame df2 для того щоб випадково не втратити наявні дані. Після чого у ньому ж створено стовпець NewOrderID, який заповнюється унікальними значеннями починаючи від 100000

![image](https://github.com/user-attachments/assets/4fff20bd-2e97-459d-9126-6c9c428c5491)

Замінюємо значення OrderID на значення із стовпця NewOrderID, після чого видаляємо останній. При перевірці на унікальність бачимо, що всі ідентифікатори унікальні

### 3) Приведення даних до правильних типів

![image](https://github.com/user-attachments/assets/34bbd4d0-dd98-449d-bb83-0880d372b701)

Помітно, що багато стовпців містять дані типу object та float, але для багатьох стовпців ці типи даних не підходять, тому замінимо їх підходящими.

#### а) Datetime

Перетворимо дані, що пов'язані із часом у формат Datetime

![image](https://github.com/user-attachments/assets/afce5b7a-a8b3-4d23-8fe7-688b0a7b75ad)

#### б) Category

Переглянемо унікальні даних у тих стовпцях, де їх мало б бути мало. 

![image](https://github.com/user-attachments/assets/cd704c61-1b09-4419-a306-ca95f5efacca)
![image](https://github.com/user-attachments/assets/0f939d92-9b89-4833-98ea-2fd231cc7ada)

Ці стовпці варто віднести до типу Category, тому що цей тип підходить для даних, де мало унікальних значень.

![image](https://github.com/user-attachments/assets/1c60da27-7fd9-44a3-9fdd-6b507a89a382)

#### в) Integer

Перетворимо дані що представляють цілі числа у тип Int64

![image](https://github.com/user-attachments/assets/420d6f3f-56b8-4384-8c41-58d6c7483685)

#### г) String

Для поштового індексу краще мати рядковий тип, оскільки поштовий індекс може містити нулі на початку

![image](https://github.com/user-attachments/assets/978dc71f-30c5-4cee-ba05-3451f3125e66)


#### Правильні типи

Тепер усі дані приведені до правильних типів

![image](https://github.com/user-attachments/assets/79330b74-74b5-4d52-b688-4df89c3fddae)

Можна помітити, що є int64 та Int64 типи даних. Різниця між ними полягає у тому, що Int64 допускає відсутність даних, а int64 ні. Це важливо у нашому випадку, тому що primary key не може мати порожніх значень.

### 4) Налаштування правильної локації

Подивимось на стовпці, що відповідають за локацію

![image](https://github.com/user-attachments/assets/1e552d95-987d-42f6-bce1-4f293c842bab)

Як бачимо, значення не співпадають між собою, тому потрібно змінити їх так, щоб усе було правильно

#### а) Заповнення порожніх значень
Спершу додамо до City категорію Unknown

![image](https://github.com/user-attachments/assets/570d545b-038d-4c12-8ca5-791b35be5bec)

Тепер заповнимо усі порожні значення

![image](https://github.com/user-attachments/assets/c9fc1ec1-49e3-4eda-a26a-384131807309)

![image](https://github.com/user-attachments/assets/2f7369a0-7add-44c2-9db3-29e090970a99)

#### б) Перевірка відповідності

Для того щоб усі значення локацій відповідали одне одному потрібно визначити словники відношень

