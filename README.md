# SQLx

Progressive SQL templates

## ENG 🇬🇧

### Motivations

Modern ORMs are often very inflexible and impose their own way of working with databases. They frequently cannot parse data not described in models (entities), forcing developers to write workarounds to handle issues with aliases and HAVING clauses in queries. Additionally, ORMs can't join tables without explicitly defined relationships between models. ORMs can generate non-optimized SQL queries, which can significantly degrade application performance.

With the variety of ORM libraries available, developers need to learn each one’s approach to working with databases. Developers often work on multiple projects that use different ORMs, which is not ideal for workflow efficiency.

Instead of having developers spend time learning a trendy new ORM, I want them to focus on what they already know—SQL. Therefore, I am launching this project to free developers from ORM-related issues and enhance their productivity in writing highly optimized SQL code.

### Name

The name stands for "SQL extended." I believe this is a good name as it suggests that the project doesn’t aim to rewrite SQL standards but only to extend them with new functionality.

### Project Goal

The goal of this project is to create a programming language for SQL query templating and an interpreter for it.

For the MVP, the SQL template engine (referred to as SQLX) should:

Support standard SQL syntax and be compatible with SQLite.
Accept arguments.
Validate templates before producing output.
Handle bindings and replacements.
Provide simple code control statements like if, else, while, switch.
Enable grouping of ON, AND, and OR operations.
Allow SQL templates to be imported from other files.
Define global aliases.
Work with standard database queries as well as stored functions and procedures.
Use JSON for parameter substitution.
Parse results into a JSON format that is not burdened by model typing—i.e., it can parse virtual fields, aliases, grouped fields, HAVING clauses, etc.
Optional:

Include a static code analyzer and formatter for .sqlx files.
An example of the syntax might look like:

```
@import 'globals.sqlx'

SELECT
  "id",
  "created_at",
  CONCAT("first_name", ' ', "last_name") AS "name",
  "role",
  "status"
FROM
  "users"
WHERE
  #and
    @if :first_name
      "first_name" = :first_name
    @if :is_admin
      "role" = 'ADMIN'
    @else if :roles
      "role" IN :roles
    @else
      "role" IN ('ADMIN', 'USER')
  #or
    @if :status
      "status" = :status
@if :order
  ORDER BY :order.col :order.type
#pagination -- Will be converted to `LIMIT :limit OFFSET :offset`
```

```typescript
import { sqlx } from "sqlx";

const result: string = await sqlx.get("query.sqlx", {
  first_name: "John",
  status: "active",
  order: {
    col: "age",
    type: "DESC",
  },
  limit: 100,
  offset: 0,
});

console.log(result); // SELECT "id", "created_at", CONCAT("first_name", ' ', "last_name") AS "name", "role", "status" FROM "users" WHERE "first_name" = "John" OR "status" = "active" ORDER BY 'age' DESC LIMIT 100 OFFSET 0;
```

## UA 🇺🇦

### Мотивація

Сучасні ORM часто дуже негнучкі та нав'язують власний спосіб роботи з базою даних. Часто вони не здатні розпарсити дані що не описані в моделях (сутностях), тож розробники мусять писати костилі щоб обійти проблему з аліасами та having в запитах. Також ОРМ не здатні джоїнити таблиці якщо не описати зв'язки між моделями. ОРМ можуть створювати неоптимізовані SQL запити що може значно погіршити продуктивність роботи застосунку.

Скільки існує бібліотек ОРМ стільки необхідно вивчити підходів до роботи з базою даних. Часто розробники паралельно працюють на декількох проєктах що мають різні ОРМ що не є оптимальним для робочого процесу.

Замість того щоб витрачати час розробників на вивчення нової трендової ОРМ я хочу щоб розробники сфокусувались на тому що вони вже знають - SQL. Тому я запускаю цей проєкт, який прикликаний позбавити розробників проблем пов'язаних з використанням ОРМ та покращити їх продуктивність в написанні дуже оптимізованого SQL коду.

### Назва

Назва розшифровується як "SQL extended". Я вважаю що це гарна назва тому що вона показує що проєкт не має наміру переписувати стандарти SQL а лише доповнити його новим функціоналом.

### Ціль проєкту

Ціль проєкту - написати мову програмування для шаблонізації SQL запитів та інтерпретатор до неї.

В рамках MVP шаблонізатор SQL (надалі SQLX) мусить:

- підтримувати стандартний синтаксис SQL та бути сумісним з SQLite
- приймати як аргумент
- валідувати шаблони перед видачею результату
- вміти працювати з bindings та replacements
- мати простий контроль виконання коду при підстановці: оператори if, else, while, switch
- мати можливість групувати операції ON, AND, OR
- можливість імпортувати шаблони SQL з інших файлів
- мати можливість описувати глобальні аліаси
- вміти працювати як зі звичайними запитами до бд, так і оперувати зі збереженими функціями та процедурами
- використовувати JSON для підстановки параметрів
- мати парсер результатів в JSON формат що не обтяжений типізацією в моделях. Тобто вміти парсити віртуальні поля, аліаси, згруповані поля, having та інші.

Необов'язково:

- мати статичний аналізатор коду та форматувальник для файлів `.sqlx`

Умовний приклад синтаксису:

```
@import 'globals.sqlx'

SELECT
  "id",
  "created_at",
  CONCAT("first_name", ' ', "last_name") AS "name",
  "role",
  "status"
FROM
  "users"
WHERE
  #and
    @if :first_name
      "first_name" = :first_name
    @if :is_admin
      "role" = 'ADMIN'
    @else if :roles
      "role" IN :roles
    @else
      "role" IN ('ADMIN', 'USER')
  #or
    @if :status
      "status" = :status
@if :order
  ORDER BY :order.col :order.type
#pagination -- Will be converted to `LIMIT :limit OFFSET :offset`
```

```typescript
import { sqlx } from "sqlx";

const result: string = await sqlx.get("query.sqlx", {
  first_name: "John",
  status: "active",
  order: {
    col: "age",
    type: "DESC",
  },
  limit: 100,
  offset: 0,
});

console.log(result); // SELECT "id", "created_at", CONCAT("first_name", ' ', "last_name") AS "name", "role", "status" FROM "users" WHERE "first_name" = "John" OR "status" = "active" ORDER BY 'age' DESC LIMIT 100 OFFSET 0;
```
