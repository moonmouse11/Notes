# Eloquent SQL Reference
***
SQL запросы, генерируемые `Eloquent ORM`.
## Get
- `get` (`all`)
```php
User::get()
```

```sql
SELECT * FROM users
```
- `get` (`all`) with soft deletes on
```php
User::get()
```

```sql
SELECT * FROM users WHERE users.deleted_at IS NULL
```
- `get` with soft deleted rows
```php
User::withTrashed()->get()
```

```sql
SELECT * FROM users
```
- `first`
```php
User::first()
```

```sql
SELECT * FROM users LIMIT 1
```
- `find`
```php
User::find(2)
```

```sql
SELECT * FROM users WHERE users.id = 2 LIMIT 1
```
- `findMany`
```php
User::findMany([1, 2, 3])
```

```sql
SELECT * FROM users WHERE users.id IN (1, 2, 3)
```
- `value`
```php
User::where('name', 'John Smith')->value('email')
```

```sql
SELECT email FROM users WHERE name = 'John Smith' LIMIT 1
```
- `paginate`
```php
User::paginate()
```

```sql
SELECT COUNT(*) AS aggregate FROM users
SELECT * FROM users LIMIT 15 OFFSET 0
```
***
## Where
- `where`
```php
User::where('age', '>', 32)->get()
```

```sql
SELECT * FROM users WHERE `age` > 32
```
- `orWhere`
```php
User::where('age', '>', 40)->orWhere('age', '<', 20)->get()
```

```sql
SELECT * FROM users WHERE age > 40 OR age < 20
```
- `whereIn`
```php
User::whereIn('age', [20, 30, 40])->get()
```

```sql
SELECT * FROM users WHERE age IN (20, 30, 40)
```
- `whereBetween`
```php
User::whereBetween('age', [20,40])->get()
```

```sql
SELECT * FROM users WHERE age BETWEEN 20 AND 40
```
- `whereDate`
```php
User::whereDate('created_at', now())->get()
```

```sql
SELECT * FROM `users` WHERE DATE(created_at) = '2023-10-04'
```
***
## Create
- `create`
```php
User::create(['name' => 'John', 'email' => 'john@smith.com'])
```

```sql
INSERT INTO users (name, email, updated_at, created_at)
    VALUES ('John', 'john@smith.com', '2023-10-04 15:30:34', '2023-10-04 15:30:34')
```
- `firstOrCreate`
```php
User::firstOrCreate(['email' => 'john@smith.com'], ['name' => 'John'])
```

```sql
SELECT * FROM users WHERE (email = 'john@smith.com') LIMIT 1
INSERT INTO users (email, name, updated_at, created_at)
    VALUES ('john@smith.com', 'John', '2023-10-04 16:17:59', '2023-10-04 16:17:59')
```
***
## Update
- `UPDATE`
```php
User::where('status', 'some_status')->update(['type' => 'some_type'])
```

```sql
UPDATE users SET type = 'some_type', users.updated_at = '2023-10-04 15:36:44'
    WHERE status = 'some_status'
```
- Update single row
```php
$user->update(['name' => 'John', 'email' => 'john@smith.com'])
```

```sql
UPDATE users SET name = 'John', email = 'john@smith.com', users.updated_at = '2023-10-04 16:12:30'
    WHERE id = 1
```
- `updateOrCreate`
```php
User::updateOrCreate(['email' => 'john@smith.com'], ['name' => 'James']);
```

```sql
SELECT * FROM users WHERE (email = 'john@smith.com') LIMIT 1
UPDATE users SET name = 'James', users.updated_at = '2023-10-04 16:14:18'
    WHERE id = 1
```
***
## Delete
- `delete`
```php
User::where('status', 'some_status')->delete()
```

```sql
DELETE FROM users WHERE status = 'some_status'
```
- Delete with soft deletes on
```php
User::where('status', 'some_status')->delete()
```

```sql
UPDATE users SET deleted_at = '2023-10-25 12:58:26', users.updated_at = '2023-10-25 12:58:26'
    WHERE status = 'some_status' AND users.deleted_at IS NULL
```
- Delete single row
```php
$user->delete()
```

```sql
DELETE FROM users WHERE id = 1
```
- Delete single row with soft deletes on
```php
$user->delete()
```

```sql
UPDATE users SET deleted_at = '2023-10-25 12:59:52', users.updated_at = '2023-10-25 12:59:52'
    WHERE id = 1
```
- `destroy`
```php
User::destroy(1, 2, 3)
```

```sql
SELECT * FROM users WHERE id IN (1, 2, 3)
DELETE FROM users WHERE id = 1
DELETE FROM users WHERE id = 2
DELETE FROM users WHERE id = 3
```
- Destroy with soft deletes on
```php
User::destroy(1, 2, 3)
```

```sql
SELECT * FROM users WHERE id IN (1, 2, 3) AND users.deleted_at IS NULL
UPDATE users SET deleted_at = '2023-10-25 12:54:53', users.updated_at = '2023-10-25 12:54:53' WHERE id = 1
UPDATE users SET deleted_at = '2023-10-25 12:54:53', users.updated_at = '2023-10-25 12:54:53' WHERE id = 2
UPDATE users SET deleted_at = '2023-10-25 12:54:53', users.updated_at = '2023-10-25 12:54:53' WHERE id = 3
```
- Restore soft deleted row
```php
$user->restore()
```

```sql
UPDATE users SET deleted_at = '', users.updated_at = '2023-10-25 13:07:24' WHERE id = 1
```
***
## One to One
`profiles` table has `id`, `user_id`, `city`
User `hasOne` profile, profile `belongsTo` user
For nested relationships examples another one to one relationship is used. Profile `hasOne` passport, passport `belongsTo` profile
- Get user's profile
```php
$user->profile or $user->profile()->first()
```

```sql
SELECT * FROM profiles WHERE profiles.user_id = 2 AND profiles.user_id IS NOT NULL LIMIT 1
```
- Get user with profile
```php
User::with('profile')->find(2)
```

```sql
SELECT * FROM users WHERE users.id = 2 LIMIT 1
SELECT * FROM profiles WHERE profiles.user_id IN (2)
```
- Get user's passport (nested one to one relationships)
```php
$user->profile->passport
```

```sql
SELECT * FROM users WHERE users.id = 2 LIMIT 1
SELECT * FROM profiles WHERE profiles.user_id = 2 AND profiles.user_id IS NOT NULL LIMIT 1
SELECT * FROM passports WHERE passports.profile_id = 4 AND passports.profile_id IS NOT NULL LIMIT 1
```
- Get user with profile AND passport (nested one to one relationships)
```php
User::with('profile.passport')->find(2)
```

```sql
SELECT * FROM users WHERE users.id = 2 LIMIT 1
SELECT * FROM profiles WHERE profiles.user_id IN (2)
SELECT * FROM passports WHERE passports.profile_id IN (4)
```
- Get users who have profiles
```php
User::has('profile')->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM profiles WHERE users.id = profiles.user_id)
```
- Get users who have passport (nested one to one relationships)
```php
User::has('profile.passport')->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM profiles WHERE users.id = profiles.user_id AND EXISTS
        (SELECT * FROM passports WHERE profiles.id = passports.profile_id)
    )
```
- Get users from Adelaide
```php
User::whereHas('profile', function ($q) {
    $q->where('city', 'Adelaide');
})->get();
```

```sql
SELECT * FROM users WHERE EXISTS
(SELECT * FROM profiles WHERE users.id = profiles.user_id AND city = 'Adelaide')
```
- Get users who have profiles and load their profiles
```php
User::has('profile')->with('profile')->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM profiles WHERE users.id = profiles.user_id)
SELECT * FROM profiles WHERE profiles.user_id IN (1, 2, 3)
```
***
## One to Many
`orders` table has `id`, `user_id`, `comment`
User has many orders. Order `belongsTo` user
For nested relationships examples another one to many relationship is used. Order `hasMany` support tickets, support ticket `belongsTo` order.
- Get user's orders
```php
$user->orders or $user->orders()->get()
```

```sql
SELECT * FROM orders WHERE orders.user_id = 1 AND orders.user_id IS NOT NULL
```
- Get users with orders
```php
User::with('orders')->get()
```

```sql
SELECT * FROM users
SELECT * FROM orders WHERE orders.user_id IN (1, 2, 3)
```
- Get users with orders and support tickets (nested one to many relationships)
```php
User::with('orders.supportTickets')->get()
```

```sql
SELECT * FROM users
SELECT * FROM orders WHERE orders.user_id IN (1, 2, 3)
SELECT * FROM support_tickets WHERE support_tickets.order_id IN (7, 8, 9)
```
- Load companies for user
```php
$user->load('orders')
```

```sql
SELECT * FROM orders WHERE orders.user_id IN (1)
```
- Get users who have orders
```php
User::has('orders')->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM orders WHERE users.id = orders.user_id)
```
 - Get users who have support tickets (nested one to many relationships)
```php
User::has('orders.supportTickets')->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM orders WHERE users.id = orders.user_id AND EXISTS
        (SELECT * FROM support_tickets WHERE orders.id = support_tickets.order_id))
```
- Get users who has orders with empty comment
```php
User::whereHas('orders', function ($q) {
    $q->whereNull('comment');
})->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM orders WHERE users.id = orders.user_id AND comment IS NULL)
```
- Get users who have orders and load their orders
```php
User::has('orders')->with('orders')->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM orders WHERE users.id = orders.user_id)
SELECT * FROM orders WHERE orders.user_id IN (1, 2, 3)
```
***
## Many to Many
`companies` table has `id`, `name`
`company_user` is a pivot table, it has `company_id`, `user_id` columns
User `belongsToMany` companies, company `belongsToMany` users
For nested relationships examples another many to many relationship is used. Company `belongsToMany` employees, employee `belongsToMany` companies
- Get user's companies
```php
$user->companies or user->companies()->get()
```

```sql
SELECT companies.*, company_user.user_id AS pivot_user_id, company_user.company_id AS pivot_company_id
    FROM companies
    INNER JOIN company_user ON companies.id = company_user.company_id
    WHERE company_user.user_id = 1
```
- Get users with companies
```php
User::with('companies')->get()
```

```sql
SELECT * FROM users
SELECT companies.*, company_user.user_id AS pivot_user_id, company_user.company_id AS pivot_company_id
    FROM companies
    INNER JOIN company_user ON companies.id = company_user.company_id
    WHERE company_user.user_id IN (1, 2, 3)
```
- Get users with employees (nested many to many relationships)
```php
User::with('companies.employees')->get()
```

```sql
SELECT * FROM users
```

```sql
SELECT companies.*, company_user.user_id AS pivot_user_id, company_user.company_id AS pivot_company_id
    FROM companies
    INNER JOIN company_user ON companies.id = company_user.company_id
    WHERE company_user.user_id IN (1, 2, 3)
SELECT employees.*, company_employee.company_id AS pivot_company_id, company_employee.employee_id AS pivot_employee_id
    FROM employees
    INNER JOIN company_employee ON employees.id = company_employee.employee_id
    WHERE company_employee.company_id IN (7, 8, 9)
```
- Load companies for user
```php
$user->load('companies')
```

```sql
SELECT companies.*, company_user.user_id AS pivot_user_id, company_user.company_id AS pivot_company_id
    FROM companies
    INNER JOIN company_user ON companies.id = company_user.company_id
    WHERE company_user.user_id IN (1)
```
- Load companies and employees for user (nested many to many relationships)
```php
$user->load('companies.employees')
```

```sql
SELECT companies.*, company_user.user_id AS pivot_user_id, company_user.company_id AS pivot_company_id
    FROM companies
    INNER JOIN company_user ON companies.id = company_user.company_id
    WHERE company_user.user_id IN (2)
SELECT employees.*, company_employee.company_id AS pivot_company_id, company_employee.employee_id AS pivot_employee_id
    FROM employees
    INNER JOIN company_employee ON employees.id = company_employee.employee_id
    WHERE company_employee.company_id IN (7, 8, 9)
```
- Get users who have companies
```php
User::has('companies')->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM companies
        INNER JOIN company_user ON companies.id = company_user.company_id
        WHERE users.id = company_user.user_id)
```
- Get users who have companies with employees (nested many to many relationships)
```php
User::has('companies.employees')->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM companies
        INNER JOIN company_user ON companies.id = company_user.company_id
        WHERE users.id = company_user.user_id AND EXISTS
            (SELECT * FROM employees
                INNER JOIN company_employee ON employees.id = company_employee.employee_id
                WHERE companies.id = company_employee.company_id))
```
- Get users who have companies with empty description
```php
User::whereHas('companies', function ($q) {
    $q->whereNull('description');
})->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM companies
        INNER JOIN company_user ON companies.id = company_user.company_id
        WHERE users.id = company_user.user_id AND description IS NULL)
```
- Get users who have companies and load their companies
```php
User::has('orders')->with('orders')->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM orders WHERE users.id = orders.user_id)
SELECT * FROM orders WHERE orders.user_id IN (1, 2, 3)
````   companies` table has `id`, `name`

`company_user` is a pivot table, it has `company_id`, `user_id` columns

User `belongsToMany` companies, company `belongsToMany` users

For nested relationships examples another many to many relationship is used. Company `belongsToMany` employees, employee `belongsToMany` companies

Get user's companies

```html
$user->companies or user->companies()->get()
```

```sql
SELECT companies.*, company_user.user_id AS pivot_user_id, company_user.company_id AS pivot_company_id
    FROM companies
    INNER JOIN company_user ON companies.id = company_user.company_id
    WHERE company_user.user_id = 1
```
- Get users with companies
```php
User::with('companies')->get()
```

```sql
SELECT * FROM users
SELECT companies.*, company_user.user_id AS pivot_user_id, company_user.company_id AS pivot_company_id
    FROM companies
    INNER JOIN company_user ON companies.id = company_user.company_id
    WHERE company_user.user_id IN (1, 2, 3)
```
- Get users with employees (nested many to many relationships)
```php
User::with('companies.employees')->get()
```

```sql
SELECT * FROM users
```

```sql
SELECT companies.*, company_user.user_id AS pivot_user_id, company_user.company_id AS pivot_company_id
    FROM companies
    INNER JOIN company_user ON companies.id = company_user.company_id
    WHERE company_user.user_id IN (1, 2, 3)
SELECT employees.*, company_employee.company_id AS pivot_company_id, company_employee.employee_id AS pivot_employee_id
    FROM employees
    INNER JOIN company_employee ON employees.id = company_employee.employee_id
    WHERE company_employee.company_id IN (7, 8, 9)
```
- Load companies for user
```php
$user->load('companies')
```

```sql
SELECT companies.*, company_user.user_id AS pivot_user_id, company_user.company_id AS pivot_company_id
    FROM companies
    INNER JOIN company_user ON companies.id = company_user.company_id
    WHERE company_user.user_id IN (1)
```
- Load companies and employees for user (nested many to many relationships)
```php
$user->load('companies.employees')
```

```sql
SELECT companies.*, company_user.user_id AS pivot_user_id, company_user.company_id AS pivot_company_id
    FROM companies
    INNER JOIN company_user ON companies.id = company_user.company_id
    WHERE company_user.user_id IN (2)
SELECT employees.*, company_employee.company_id AS pivot_company_id, company_employee.employee_id AS pivot_employee_id
    FROM employees
    INNER JOIN company_employee ON employees.id = company_employee.employee_id
    WHERE company_employee.company_id IN (7, 8, 9)
```
- Get users who have companies
```php
User::has('companies')->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM companies
        INNER JOIN company_user ON companies.id = company_user.company_id
        WHERE users.id = company_user.user_id)
```
- Get users who have companies with employees (nested many to many relationships)
```php
User::has('companies.employees')->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM companies
        INNER JOIN company_user ON companies.id = company_user.company_id
        WHERE users.id = company_user.user_id AND EXISTS
            (SELECT * FROM employees
                INNER JOIN company_employee ON employees.id = company_employee.employee_id
                WHERE companies.id = company_employee.company_id))
```
- Get users who have companies with empty description
```php
User::whereHas('companies', function ($q) {
    $q->whereNull('description');
})->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM companies
        INNER JOIN company_user ON companies.id = company_user.company_id
        WHERE users.id = company_user.user_id AND description IS NULL)
```
- Get users who have companies and load their companies
```php
User::has('orders')->with('orders')->get()
```

```sql
SELECT * FROM users WHERE EXISTS
    (SELECT * FROM orders WHERE users.id = orders.user_id)
SELECT * FROM orders WHERE orders.user_id IN (1, 2, 3)
```
***
## Aggregate functions
- `count`
```php
User::count()
```

```sql
SELECT COUNT(*) AS aggregate FROM users
```
- `max` (`min`, `avg`, `sum`)
```php
User::max('age')
```

```sql
SELECT MAX(age) AS aggregate FROM users
```
- `withSum` (`withAvg`, `withMin`, `withMax`, `withCount`)
```php
User::withSum('orders', 'total')->get();
```

```sql
SELECT users.*, (SELECT SUM(orders.total) FROM orders WHERE users.id = orders.user_id) AS orders_sum_total FROM users
```
***
## Miscellaneous
- `latest` (`oldest`)
```php
User::latest()->get()
```

```sql
SELECT * FROM users ORDER BY created_at DESC
```
- `withExists`
```php
User::withExists('orders')->get()
```

```sql
SELECT users.*, EXISTS(SELECT * FROM orders WHERE users.id = orders.user_id) AS orders_exists FROM users
```
- `increment` (`decrement`)
```php
User::where('id', 2)->increment('bonus', 12)
```

```sql
UPDATE users SET bonus = bonus + 12, users.updated_at = '2023-10-25 14:58:10' WHERE id = 2
```
