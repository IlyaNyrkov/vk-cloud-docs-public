PostGIS — расширение объектно-реляционной СУБД PostgreSQL предназначенное для хранения в базе географических данных. PostGIS включает поддержку пространственных индексов R-Tree/GiST и функции обработки геоданных, а также опциональные расширения для работы с адресами и топологией.

После добавления этого расширения в базу можно работать с ней специализированными запросами. Полный перечень запросов и другую полезную информацию можно прочитать на [официальном ресурсе](https://gis-lab.info/docs/postgis/manual/postgis-manual-ru-1.3.4.pdf).

### Доступные виды геометрических объектов

Вам доступны: point, line, polygon, multipoint, multiline, multipolygon, and geometrycollections (точка, линия, полигон, мультиточка, мультилиния, мультиполигон и геометрическая коллекция). Они определены в формате Well Known Text Open GIS с расширениями XYZ, XYM, XYZM.

### Как построить пространственный запрос?

Чтобы построить пространственный запрос, вы должны создать таблицу со столбцом типа _geometry_, который будет содержать ваши ГИС-данные. Соединитесь с вашей базой данных с помощью PSQL и выполните SQL:

```
             CREATE TABLE gtest ( ID int4, NAME varchar(20) ); SELECT AddGeometryColumn('', 'gtest','geom',-1,'LINESTRING',2);
```

Если не получилось добавить столбец геометрии, то, вероятно, вы не загрузили функции и объекты PostGIS в свою базу данных. Смотрите инструкцию по установке.

Далее, вы можете вставлять геометрию в таблицу с помощью SQL-команды _insert_. Объект ГИС будет форматирован согласно формату well-known text Консорциума OpenGIS:

```
INSERT INTO gtest (ID, NAME, GEOM) VALUES ( 1, 'Первая геометрия', GeomFromText('LINESTRING(2 3,4 5,6 5,7 8)', -1) );
```

Подробную информацию о других объектах ГИС можно посмотреть в справочнике объектов. Просмотр ваших ГИС-данных в таблице:

```
             SELECT id, name, AsText(geom) AS geom FROM gtest;
```

Результат должен выглядеть примерно так:

```
id | name | geom ---+------------------+-----------------------------        1 | Первая геометрия | LINESTRING(2 3,4 5,6 5,7 8)       (1 row)
```

### Как вставить объект ГИС в базу данных?

Так же, как вы строите другие запросы к базе данных, используя SQL-запрос для получения значений, функций, логических тестов.

При построении пространственных запросов следует учитывать:

- существует ли пространственный индекс, которым можно воспользоваться;
- нужно ли произвести сложные вычисления на большом числе геометрий.

Чаще всего вам будет нужен «оператор пересечения» (&&), который проверяет пересекаются ли границы объектов. Польза оператора && заключается в том, что он может использовать пространственный индекс, если он существует. Это ускорит выполнения запроса.

Кроме того, вы можете использовать пространственные функции, такие как Distance(), ST_Intersects(), ST_Contains() and ST_Within() и другие для сужения результатов поиска. Большинство пространственных запросов включают тест на индекс и тест на пространственную функцию. Тест индекса полезен тем, что ограничивает число проверок значений теми, которые могут попасть в требуемый набор. Далее используются пространственные функции для окончательной проверки условия.

```
             SELECT id, the_geom FROM thetable              WHERE              the_geom && 'POLYGON((0 0, 0 10, 10 10, 10 0, 0 0))'              AND              _ST_Contains(the_geom,'POLYGON((0 0, 0 10, 10 10, 10 0, 0 0))');
```

#### Таблица дополнений

|Название|Описание|
|---|---|
|[`address_standardizer`](https://postgis.net/docs/manual-2.5/Address_Standardizer.html)|Нормализатор адресов.|
|[`address_standardizer_data_us`](https://postgis.net/docs/manual-2.5/Address_Standardizer.html)|Нормализатор адресов для США.|
|[`postgis_tiger_geocoder`](https://postgis.net/docs/manual-2.5/Extras.html)|Дополнение для работы с TIGER (Topologically Integrated Geographic Encoding and Referencing system).|
|`postgis_topology`|Дополнение для работы с топологией — вершинами, гранями, полигонами и т.д.|
|[`pgrouting`](https://pgrouting.org/)|Дополнение для поиска кратчайшего пути.|

#### Параметры, применимые в инфраструктуре VK Cloud:

|Название|Описание|
|---|---|
|`database`|Перечень баз данных, на которых нужно развернуть расширение. Удаление баз данных из этого списка для установленного расширения не поддерживается.|
|`extension_list`|Список дополнений, которые надо включить (по умолчанию пусто). Удаление параметров из этого списка для установленного расширения не поддерживается.|
