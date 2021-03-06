## Аргументы и типы

Класс также как и примитивы определяет тип данных, но **более высокого
уровня**. Поэтому объект класса `Animal` принадлежит как примитивному типу `object`, так и типу класса `Animal`.В этом разделе мы
рассмотрим обе разновидности типов данных по отношению к методам класса. Если вы не хотите неожиданных результатов при вызове метода, который
по вашей задумке должен принимать объект определенного типа (например, вашего класса), то можно
задать тип напрямую.

```php
public function run(Animal $dog) {
    //...
}
// Тут мы явно задали тип аргумента, при попытке передать значение другого типа, получим ошибку TypeError
```

Начиная с **PHP 7** предоставляется возможность объявления скалярных типов. Это позволяет вам
обеспечить передачу в списке аргументов логическими, строковыми, целочисленными типами и типами с плавающей точкой.
Попробуем объявить тип данных в конструкторе:
```php
class Animal {
    public $name;
    public $color;
    public $age = 0;
    public function __construct(string $name, string $color, int $age)
    {
        $this->name = $name;
        $this->color = $color;
        $this->age = $age;
    }
    // ...
}
```
Теперь, если мы попытаемся создать экземпляр класса, и при этом передать значения неверного типа,
мы получим `TypeError`.

### Смешанные типы
Объявление смешанного типа, введенное в **РНР 8.0**, может рассматриваться как пример “синтаксического сахара”, который сам по себе мало что
дает.
```php
class Storage
{
    public function add(string $key, $value)
    {
        // Действия c $ke   y и $value
    }
}

class Storage
{
    public function add(string $key, mixed $value)
    {
        // Действия c $key и $value
    }
}
// Никакой функциональной разницы нет, мы лишь показали что мы СПЕЦИАЛЬНО сделали $value гибким.
```
### Объединения
Если мы хотим принимать тот или иной конкретный тип в виде аргумента (например, аргумент
должен быть **строкой или булевым значением**). Можно передавать сколько угодно типов, а также тип класса.
```php
class Storage {
    public function add(string $key, string|bool|ShopItem $value)
    {
        // Действия c $key и $value
    }
} 
//При попытке передать значение другого типа, получим ошибку TypeError
```

### Типы, принимающие значение null
Если передаваемый аргумент при каких-либо обстоятельствах может иметь значение `null`, и нас это устраивает,
мы можем сделать "проверку на `null`".
```php
class Storage {
    public function add(string $key, ?string $value)
    {
        // Действия c $key и $value
    }
} 
//Если $value не null, значение будет иметь тип string и наоборот
```
### Объявление возвращаемого типа
Мы также можем явно указать, значение какого типа возвращает наша функция, вот пример:
```php
class Test {
    public $length;
    public function getLength() : int
    {
        return $this->length;
    }
}
// Функция всегда должна возвращать int, если будет другой тип, получим TypeError
```
**Также как и с аргументами методов, мы можем делать объединения и проверки на** `null`.

Начиная с **PHP 8**, имеется один тип, который поддерживается объявлениями возвращаемого типа, но не типов аргументов. Вы можете объявить, что метод никогда не вернет никакого значения, используя псевдотип
`void`. Так, например, поскольку метод `setDiscount()` предназначен только для установки значения, но не для его возврата, я использую объявление возвращаемого типа `void`:
```php
class Test {
    public $length;
    public function setDiscount(int|float $num): void
    {
        $this->discount = $num;
    }
}
// Функция ничего не возвращает
```

### Подводя итоги, в таблице ниже перечислены объявления типов в **PHP**

| Объявление типа | Начиная с версии | Описание |
| ----------- | ------------- | ---------- |
| array       | 5.1   | Массив. По умолчанию может быть null-значением или массивом |
| int         | 7.0   | Целое значение. По умолчанию может быть null или целым значением |
| float       | 7.0   | Числовое значение с плавающей (десятичной) точкой. Допускается целое значение, даже если включен строгий режим. По умолчанию может быть null, целым или числовым значением с плавающей точкой       |
| callable    | 5.4   | Вызываемый код (например, анонимная функция). По умолчанию может быть null       |
| bool        | 7.0   | Логическое значение. По умолчанию можетбыть null или логическим значением       |
| string      | 5.0   | Символьные данные. По умолчанию может быть null или строковым значением       |
| self        | 5.0   | Ссылка на содержащий класс       |
| [class type]| 5.0   | Тип класса или интерфейса. По умолчанию может быть null       |
| iterable    | 7.1   | Может быть обойден с использованием цикла foreach (необязательно массив)       |
| object      | 7.2   | Объект       |
| mixed       | 8.0   | Явное указание, что значение может быть любого типа       |