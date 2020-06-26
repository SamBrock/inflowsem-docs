# Database

The INFLOWSEM database is hosted on asteria, same as the website.

To manage the database, read the data and test SQL statements, go to [phpMyAdmin](http://asteria.hud.ac.uk/phpMyAdmin). The INFLOWSEM database (technically just a table within an already established database) can be found in the database named `www`.

The table only holds data about the booked consultations. If you need additional tables, you may want to rename the `INFLOWSEM` table to something like `INFLOWSEM_consultation`.

## Connecting to the Database

The site connects to the database using PDO. The connection to the database can be found in `src/database/database.php`.

To execute a SQL statement, you need to get the connection to the database:

```php
$conn = getConnection();
```
You should also close the connection once executed:

```php
closeConnection($conn);
```

## Querying the database

There's currently 2 functions in `src/database/database.php` which query the database:
- `getConsultationDates()` returns an array of consultation dates. 
- `addConsultationDate($email, $date)` inserts the email and consultation date and time into the database. 

These functions are accessed differently throughout the site.
- `getConsultationDates()` has an additional file (`src/database/get-consultation-dates.php`) which runs the function and returns the dates array, encoded as JSON. This can gotten from an AJAX request and returned to the JS file for disabling dates / times in the consultation booking calendar. 
- `addConsultationDate($email, $date)` is ran in `src/process-form.php` after the consultation form is processed. 

Each of these functions are not ran directly in `database.php`. Instead, they are the database file, with it's database connection and functions, are 'required' using the `require_once` statement in PHP. This allows you to use the functions anywhere within a PHP file, and more than once.