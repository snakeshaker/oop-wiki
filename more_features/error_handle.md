## Обработка ошибок

Трейты напоминают классы, экземпляры которых нельзя
получить, но можно включить в другие классы. Поэтому любое свойство
(или метод), определенное в трейте, становится частью того класса, в который включен этот трейт. При этом трейт изменяет структуру данного
класса, но не меняет его тип. (Насколько вы помните, классы и интерфейсы меняют тип).

```php
trait PriceUtilities
{
    private $taxrate = 20;
    public function calculateTax(float $price): float
    {
        return (($this->taxrate / 100) * $price);
    }
    // Другие служебные методы
}

class ShopProduct
{
    use PriceUtilities; // используем трейт (весь код трейта в виде одной строки)
}
```
