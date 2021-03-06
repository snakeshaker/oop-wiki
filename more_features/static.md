## Статические методы и свойства

Доступ к методам и свойствам можно получать **в контексте класса**, а не
объекта. Такие методы и свойства являются **статическими** и
должны быть объявлены с помощью ключевого слова `static`, как показано ниже:

```php
class StaticExample
{
    public static int $aNum = 0;
    public static function sayHello(): void
    {
        print "Здравствуй, Мир!";
    }
}
```

*Статические методы* — это функции, применяемые в контексте класса.
Они **не могут сами получать доступ к обычным свойствам класса**, потому что такие свойства относятся к объектам. Но **из статических методов
можно обращаться к статическим свойствам**.Если изменить статическое
свойство, то все экземпляры данного класса смогут получить доступ к новому значению этого свойства.

Чтобы получить доступ к статическому методу или свойству из того же самого
класса (а не из дочернего класса), мы пользуемся ключевым словом `self`.
Ключевое слово `self` служит для обращения к текущему классу подобно
тому, как псевдопеременная `$this` — к текущему объекту.

    Вызов метода с помощью ключевого слова parent — это единственный случай, когда следует использовать статическую ссылку на нестатический метод.

### Константные свойства
Константные свойства могут содержать только значения, относящиеся
к **примитивному типу**. Константе **нельзя присвоить объект**. Как и к статическим свойствам, доступ к константным свойствам осуществляется через
класс, а не через экземпляр объекта. А поскольку константа определяется **без знака доллара**, при обращении к ней не нужно указывать никакого
специального знака перед ее именем:
```php
class ConstExample
{
    const INFO = 1;
}
print ConstExample::INFO; // 1
```

### Позднее статическое связывание: ключевое слово static
Если мы к примеру хотим создать экземпляр класса внутри метода класса, мы напишем такой код.

```php
class DomainObject 
    {
    public static function create(): DomainObject
    {
        return new self();
    }
}
```

Но если попытаться выполнить метод в наследуемом классе, мы получим ошибку. Так как
`self` ссылается на класс, где метод **был создан, а не вызван**. Чтобы сделать ссылку
именно **на класс, где метод вызывается**, используем ключевое слово `static`.
```php
abstract class DomainObject
{
    public static function create(): DomainObject
    {
        return new static();
    }
}

class User extends DomainObject {}

print_r(User::create()); // Создастся объект типа User, ошибки нет
```
