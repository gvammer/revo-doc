# Рево Рассрочка: Плагин для 1С Битрикс 

Модуль позволяет подключить платежную систему Оплата Частями Рево к интернет-магазину битрикс. 

```
!!! Важно: модуль требует расширения php-curl на сайте
```

## Платежная система

Модуль при установке добавляет платежную систему "Оплата Частями" в интернет-магазин битрикс.
При выборе платежной системы в стандартном компоненте оформления заказа модуль 
открывает окно регистрации в РЕВО.

При оформлении заказа и переходе к оплате модуль открывает окно оформления заказа в РЕВО.

Оплатить в рассрочку на 12 месяцев возможно не более 100% и не менее 20% от заказа (параметр задан в настройках модуля).

Минимальная сумма заказа должна быть при этом не менее 3 000 руб. 

При этом дополнительно в настройках модуля можно указать максимальную сумму товара для отображения ссылки на детельноый стренице.

Оплата производится посредством вызова модального окна оформления рассрочки.

Система проверяет, был ли у пользователя оформлен кредитный лимит. 
Если нет, перед оформлением заказа
пользователю показывается окно получения лимита.

## Компонент Товар Детально

В стандартном компоненте детальной страницы товара модуль добавляет ссылку "Оформить рассрочку".
При нажатии на ссылку открывается окно получения лимита в РЕВО.

## Компонент Корзина товаров

В стандартный компонент "Корзина товаров" модуля "Интернет-магазин" автоматически добавляется 
ссылка оформления заказа в рассрочку.

![Сниппет в корзине](https://i.snag.gy/X6Rc3g.jpg)

## Финализация заказа

Для подтверждения заказа после одобрения и оплаты через РЕВО необходимо
перевести заказ в статус "Выполнен". При этом в РЕВО автоматически будет доставлен чек заказа для подтверждения выдачи рассрочки.

## Сниппет и компонент

Модуль предоставляет для работы с заказом сниппет и компонент "Оформить заказ в рассрочку". 
Сниппет и компонент реализуют ссылку для оформления товара в рассрочку. ID товара и ежемесячный платеж указываются 
в настройках компонента или в соответствующем тексте сниппета. Для автоматического добавления товара и перехода в корзину 
необходимо указать ID товара, его можно взять из родительского компонента или из глобального массива **\$_REQUEST**.

_! После установки модуля необходимо обновить кэш списка компонентов и сниппетов в визуальном редакторе_

![Компонент](https://i.snag.gy/xIP2tJ.jpg)

![Сниппет](https://i.snag.gy/jU8qkD.jpg)

## Установка компонента в шаблон другого компонента

Поскольку компонент ссылки оформления рассрочки зависит от конкретного товара, лучше всего
его внедрять в шаблон компонента "catalog.element" или его аналога на вашем сайте.

Для внедрения компонента необходимо добавить в место выведения ссылки следующий код:

```php

<?$APPLICATION->IncludeComponent(
	"revo:buy.link",
	"",
	Array(
		"PRICE" => $arResult['ITEM_PRICES'][0]['BASE_PRICE'],
		"BUY_BTN_SELECTOR" => '#' . $itemIds['BUY_LINK']
	)
);?>


```

Компонент принимает два параметра 

- исходную цену, от которой будет расчитана ежемесячная оплата
- селектор кнопки оформления заказа. Скрипт автоматически произведет клик на нее после получения лимита.

## Настройка печатной формы

Для отправки в Рево данных необходимо настроить печатную форму. 
Это можно сделать на странице настройки платежной системы "Оплата Частями" в разделе

`Магазин > Настройки > Платежные системы`.

## Данные для тестирования

ID магазина: **204**
Пароль: **6279a164f5cb8bbe93f7**