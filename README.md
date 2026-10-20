# Nest Care — Full Laravel + SQLite

This version runs as a **single Laravel application**: Laravel routes, controllers, Blade templates, CSS, session authentication, Eloquent models and SQLite storage. The separate React/Vite frontend has been removed. It creates **no demo users, nurseries, rooms, children, or attendance records**.

## Requirements
- PHP 8.2+ with `pdo_sqlite` and `sqlite3` enabled
- Composer 2+

## Run on Windows / XAMPP / VS Code

Open a terminal in `backend` (the Laravel project root):

```powershell
composer install
Copy-Item .env.example .env
php artisan key:generate
New-Item -ItemType File -Force database/database.sqlite
php artisan migrate
php artisan serve
```

If `.env` already exists, do not overwrite it without backing it up. Open `http://127.0.0.1:8000`.

For XAMPP, make sure `extension=pdo_sqlite` and `extension=sqlite3` are enabled in the PHP `php.ini` used by your terminal, then restart the terminal. Verify with `php -m`.

## First use
1. Choose **Create a workspace**. This creates a tenant and a manager account only.
2. Add a nursery, then create rooms and children under that nursery.
3. The manager can create staff accounts and guardian accounts. Guardian accounts must be linked to one child.
4. Staff can manage nurseries, rooms, children, and attendance, but cannot manage user accounts or delete managers.
5. Sign in as a guardian to see only that linked child's current status and attendance history. Guardian accounts are read-only.

There are no default manager credentials. All operational data is entered through the application and stored in SQLite.

## Security and business rules
- Every data query and mutation is scoped to the signed-in user's tenant; browser-supplied tenant IDs are not trusted.
- A tenant can have multiple nurseries; rooms and children are associated with a nursery.
- A child can only check into a room in their assigned nursery and can have one active check-in at a time.
- Guardian accounts can access only their linked child's attendance.
- Only managers can create/delete user accounts. A manager cannot delete their own account or another manager through this UI.
- Attendance history is retained: rooms and children with attendance history cannot be deleted; children with guardian accounts cannot be deleted; nurseries with rooms or children cannot be deleted.
- Laravel's existing JSON API remains available under `/api` for integrations; the main UI is Blade and does not require Node.js.

## Tests
```powershell
php artisan test
```

## Database reset warning
To intentionally start from an empty local database, stop the server, back up and then remove `database/database.sqlite`, recreate it, and run `php artisan migrate`. This permanently deletes records in that file. Never do this if it contains data you need.


# Credentials for existing Users

```Admin Login Details (Manager)
User: admin@gmail.com
Pass: Admin@123456

Staff
User: staff@gmail.com 
Pass: Staff@123456

Parents 
User: parent1@gmail.com
Pass: Parents1@123456

Parents2
User: parent2@gmail.com
Pass: Parents2@123456
