# Резидентна база даних Redis
## Резидентні бази даних
Резидентні бази даних — це різновид систем керування базами даних, який покладається на оперативну пам’ять для зберігання комп’ютерних даних.

## Особливості
Основна особливість резидентних баз даних — швидкість, яка досягається за рахунок використання оперативної пам'яті комп'ютера, швидкість читання та запису якої значно перевершує дискові чи твердотілі носії. Це дозволяє значно скоротити затримки при виконанні запитів до бази даних.

Використання оперативної пам’яті також призводить до волатильності даних. Зокрема, у разі втрати живлення, дані, що зберігаються в енергозалежній оперативній пам'яті, втрачаються. Втім, деякі сучасні резидентні бази даних пропонують різні механізми збереження даних на диск.

## Основні сценарії використання
* Кешування: під час створення високопродуктивних сервісів часто використовується кеш — високошвидкісне сховище для зберігання деякої підмножини даних основної бази даних. Основне призначення кеша полягає в тому, щоб зменшити потребу в доступі до основної бази даних з гіршою продуктивністю оброблення запитів.

* Сесійні сховища: застосунки часто використовують сховища сесій для відстеження ідентичності користувача, облікових даних, персоналізованої інформації, останніх дій тощо. Застосунок читає та записує ці дані лише до сховища сеансів, тому швидкість, на відміну від довговічності, є критичною.

## Redis
Redis — це резидентна база даних «ключ-значення» з відкритим кодом. Redis є найпопулярнішою NoSQL базою даних і однією з найпопулярніших баз даних в цілому та використовується в продуктах більшості великих компаній.

## Бази даних «ключ-значення»
База даних «ключ-значення» використовують асоціативні масиви (також відомі як словники) для зберігання даних. Вони містять ключ — рядок, який завжди унікальний, і значення — довільно велике поле даних, яке відповідає ключу. Їх легко розробити та реалізувати, проте вони суттєво відрізняються від реляційних баз даних, які організовують дані в таблиці та визначають зв’язки між ними.

## Встановлення Redis
Redis можна нативно встановити на таких платформах, як Linux та MacOS. На операційній системі Windows Redis не підтримується офіційно, однак його можна використовувати через WSL.

### Встановлення на MacOS:  
Спершу переконайтеся, що у вас встановлено Homebrew. У терміналі виконайте:
```
brew --version
```
Якщо команда не працює, встановіть [Homebrew](https://brew.sh/).

Далі виконайте наступну команду:
```
brew install redis
```
Вона встановить Redis на вашу систему.

### Встановлення на Red Hat / Rocky Linux:  
У терміналі виконайте:
```
sudo dnf install redis
sudo systemctl enable redis
sudo systemctl start redis
```

## Структури даних Redis
Redis пропонує безліч структур для зберігання та організації даних.

Найбільш розповсюджені з них:

* Рядки 
* Геші
* Сети
* Списки

### Базові команди

```
SET key value
```
Опис: Створює пару ключ-значення. Часова складність: O(1)

```
GET key
```
Опис: Отримати значення, що відповідає ключу. Часова складність: O(1)

```
DEL key [key ...]
```
Опис: Видаляє ключі. Часова складність: O(N)

```
KEYS pattern
```
Опис: Повертає всі ключі, що відповідають патерну. Часова складність: O(N)

```
EXISTS key [key ...]
```
Опис: Перевіряє наявність одного чи кількох ключів. Часова складність: O(N)

### Час існування

```
TTL key
```
Опис: Повертає залишковий час існування ключа. Часова складність: O(1)

```
PERSIST key
```
Опис: Видаляє час існування ключа. Часова складність: O(1)

```
EXPIRE key seconds
```
Опис: Встановлює час існування ключа, після якого ключ буде автоматично видалено. Часова складність: O(1)

```
SETEX key seconds value
```
Опис: Створює пару ключ-значення з часом існування. Часова складність: O(1)

### Геші
```
HSET key field value [field value ...]
```
Опис: Встановлює для вказаних полів відповідні значення в геші, що зберігається в ключі. Часова складність: O(N)

```
HGET key field
```
Опис: Повертає значення, пов’язане з полем у геші, що зберігається в ключі. Часова складність: O(1)

### Сети
```
SADD key member [member ...]
```
Опис: Додає вказані елементи до сету, що зберігається в ключі. Часова складність: O(N)

```
SREM key member [member ...]
```
Опис: Видаляє вказані елементи з сету, що зберігається в ключі. Часова складність: O(N)

```
SISMEMBER key member
```
Опис: Перевіряє наявність елемента сету. Часова складність: O(1)

```
SCARD key
```
Опис: Повертає кількість елементів сету, що зберігається в ключі. Часова складність: O(1)

### Списки
```
LPUSH key value [value ...]
```
Опис: Вставляє вказані значення в початок списку, що зберігається в ключі. Часова складність: O(N)

```
RPUSH key value [value ...]
```
Опис: Вставляє вказані значення в кінець списку, що зберігається в ключі. Часова складність: O(N)

```
LRANGE key start stop
```
Опис: Повертає вказаний проміжок списку, що зберігається в ключі. Часова складність: O(S+N), де S — початкова відстань, а N — кількість елементів у вказаному діапазоні.

```
LLEN key
```
Опис: Повертає довжину списку, що зберігається в ключі. Часова складність: O(1)

```
LPOP key [count]
```
Опис: Вилучає та повертає перший елемент списку, що зберігається в ключі. Часова складність: O(N)

```
RPOP key [count]
```
Опис: Вилучає та повертає останній елемент списку, який зберігається в ключі. Часова складність: O(N)

### Висновок
В рамках даної доповіді ми розглянули принцип роботи та сценарії використання резидентних баз даних на прикладі найбільш популярної резидентної бази даних Redis.

Ми також ознайомилися з базовими командами, що використовуються для взаємодії з Redis, зокрема дослідили основні структури даних, які підтримує Redis, такі як рядки, геші, сети та списки. 

Я сподіваюся, що ці знання допожуть Вам краще зрозуміти, яким чином Redis може бути використаний для ефективного зберігання та швидкої обробки даних у вашому наступному проєкті.

### Джерела
1. [Офіційний сайт Redis](https://redis.io/)
2. [Офіційна документація Redis](https://redis.io/docs/latest/)
