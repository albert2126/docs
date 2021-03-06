# Типы данных

В {{ ydb-short-name }} используются типы данных YQL. Некоторые из типов YQL поддерживаются с ограничениями — они могут использоваться только в вычислениях, но не могут быть типом столбца или использоваться в первичном ключе. Все столбцы, включая ключевые, могут содержать специальное значение NULL.

{% note alert %}

Несмотря на возможность в части полей составного первичного ключа хранить NULL, мы настоятельно рекомендуем никогда так не делать.

{% endnote %}

В таблицах ниже приведены возможные варианты использования типов данных YQL в {{ ydb-short-name }}.

## Числовые {#numeric}

| Тип    | Пояснение      | Используется в<br/>запросах и<br/>вычислениях<br/>YQL | Используется в<br/>качестве типа<br/>данных столбца | Используется в<br/>первичных<br/>ключах | Поддерживает<br/>возможность<br/>сравнения |
| ----- | ----- | ----- | ----- | ----- | ----- |
| **Bool** | Булевое значение | Да | Да | Да | Да |
| **Int8** | Целое число со знаком (от –2<sup>7</sup> до 2<sup>7</sup>–1) | Да | Нет | Нет | Да |
| **Int16** | Целое число со знаком (от –2<sup>15</sup> до 2<sup>15</sup>–1) | Да | Нет | Нет | Да |
| **Int32** | Целое число со знаком (от –2<sup>31</sup> до 2<sup>31</sup>–1) | Да | Да | Да | Да |
| **Int64** | Целое число со знаком (от –2<sup>63</sup> до 2<sup>63</sup>–1) | Да | Да | Да | Да |
| **Uint8** | Беззнаковое целое число (от 0 до 2<sup>8</sup>–1) | Да | Да | Да | Да |
| **Uint16** | Беззнаковое целое число (от 0 до 2<sup>16</sup>–1) | Да | Нет | Нет | Да |
| **Uint32** | Беззнаковое целое число (от 0 до 2<sup>32</sup>–1) | Да | Да | Да | Да |
| **Uint64** | Беззнаковое целое число (от 0 до 2<sup>64</sup>–1) | Да | Да | Да | Да |
| **Float** | Число с плавающей точкой | Да | Да | Нет | Да |
| **Decimal** | Число фиксированной точности, поддерживается **Decimal(22,9)** — 13 знаков в целой части, 9 в дробной | Да | Да | Нет | Да |
| **Double** | Число двойной точности | Да | Да | Нет | Да |

## Строковые {#string}

| Тип    | Пояснение      | Используется в<br/>запросах и<br/>вычислениях<br/>YQL | Используется в<br/>качестве типа<br/>данных столбца | Используется в<br/>первичных<br/>ключах | Поддерживает<br/>возможность<br/>сравнения |
| ----- | ----- | ----- | ----- | ----- | ----- |
| **String** | Может содержать произвольные бинарные данные | Да | Да | Да | Да |
| **Utf8** | Содержит текст в&nbsp;кодировке UTF-8 | Да | Да | Да | Да |
| **Json** | Содержит JSON | Да | Да | Нет | Нет |
| **Yson** | Содержит YSON | Да | Да | Нет | Нет |
| **Uuid** | Содержит UUID | Да | Нет | Нет | Да |

Максимальный размер значения в ячейке с любым строковым типом данных — около 4 МБ.

## Дата и время {#datetime}

| Тип    | Пояснение      | Используется в<br/>запросах и<br/>вычислениях<br/>YQL | Используется в<br/>качестве типа<br/>данных столбца | Используется в<br/>первичных<br/>ключах | Поддерживает<br/>возможность<br/>сравнения |
| ----- | ----- | ----- | ----- | ----- | ----- |
| **Date** | Точность до дней | Да | Нет | Нет | Да |
| **Datetime** | Точность до секунд | Да | Нет | Нет | Да |
| **Timestamp** | Точность до микросекунд | Да | Нет | Нет | Да |
| **Interval** | Точность до микросекунд, допустимы значения интервалов — не более 24 часов | Да | Нет | Нет | Да |
| **TzDate** | Точность до дней, содержит метку временной зоны | Да | Нет | Нет | Да |
| **TzDatetime** | Содержит метку временной зоны, точность до секунд | Да | Нет | Нет | Да |
| **TzTimestamp** | Содержит метку временной зоны, точность до микросекунд | Да | Нет | Нет | Да |

## Опциональные (nullable) {#nullable}

Любые типизированные данные в YQL, включая столбцы таблиц, бывают как гарантированно имеющие значение, так и потенциально пустые (что обозначается как `NULL`). Такие значения называют «опциональными» или в терминах SQL «nullable».

Наиболее распространённой операцией для таких типов данных является COALESCE, позволяющий оставить заполненные значения без изменений, а `NULL` заменить на указанное следом значение по умолчанию.

В текстовом виде опциональные типы выделяются вопросительным знаком в конце (например, `String?`) или как `Optional<...>`.

## Контейнеры {#containers}

| Название | Объявление&nbsp;типа | Пример&nbsp;типа | Пояснение |
| ----- | ----- | ----- | ----- |
| Список | `List<Type>` | `List<Int32>` | Последовательность переменной длины, состоящая из элементов одного типа |
| Словарь | `Dict<KeyType,ValueType>` | `Dict<String,Int32>` | Набор пар ключ—значение с фиксированным типом ключей и значений |
| Кортеж | `Tuple<Type1,...,TypeN>` | `Tuple<Int32,Double>` | Набор безымянных элементов фиксированной длины с указанными типами всех элементов |
| Структура | `Struct<Name1:Type1,...,NameN:TypeN>` | `Struct<Name:String,Age:Int32>` | Набор именованных полей с указанными типами значений, фиксированный на момент начала запроса (то есть обязательно не зависящий от данных) |
| Поток | `Stream<Type>` | `Stream<Int32>` | Однопроходной итератор по значениям одного типа. Не является сериализуемым. |
| Вариант над кортежем | `Variant<Type1,Type2>` | `Variant<Int32,String>` | Кортеж, про который известно, что заполнен ровно один элемент. |
| Вариант над структурой | `Variant<Name1:Type1,Name2:Type2>` | `Variant<value:Int32,error:String>` | Структура, про которую известно, что заполнен ровно один элемент. |

При необходимости контейнеры можно вкладывать друг в друга в произвольных комбинациях, например `List<Tuple<Int32,Int32>>` (список, содержащий в качестве элементов кортежи).

Опциональные значения в некоторых контекстах также могут рассматриваться как один из видов контейнеров (`Optional<Type>`), который ведёт себя как список длины 0 или 1.

Для представления множеств следует использовать словарь с значениями типа `Void` — `Dict<T, Void>`.


