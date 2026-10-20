# Full Laravel + SQLite

Nest Care is a nursery management application built as a **single Laravel application**. It uses Laravel routes, controllers, Blade templates, CSS, session-based authentication, Eloquent models, and SQLite for database storage.

The separate React/Vite frontend has been removed. The application does **not** create demo users, nurseries, rooms, children, or attendance records automatically.

## Technology Stack

- **Backend:** PHP and Laravel
- **Frontend:** Laravel Blade, HTML, CSS
- **Database:** SQLite
- **ORM:** Laravel Eloquent
- **Authentication:** Session-based authentication
- **API:** Laravel JSON API under `/api`
- **Development tools:** Composer, XAMPP, and VS Code

## AI-Assisted Development

ChatGPT was used as an assistance tool during development for selected aspects of the project, including:

- **UI/UX design:** Assistance with the application layout, interface structure, and usability improvements.
- **Algorithm and business logic:** Assistance with planning and refining workflows, validation rules, attendance processing, and role-based access logic.
- **Framework and architecture:** Assistance with evaluating the Laravel application structure, organizing components, and selecting suitable implementation approaches.
- **Troubleshooting and code review:** Assistance with identifying potential logical issues and improving implementation quality.

The application remains a software development project that should be understood, tested, and validated by its developer. AI assistance does not replace understanding the code, verifying business rules, or testing the application.

## Requirements

- PHP 8.2 or later, with `pdo_sqlite` and `sqlite3` enabled
- Composer 2 or later

## Run on Windows / XAMPP / VS Code

Open a terminal in `backend`, which is the Laravel project root.

```powershell
composer install
Copy-Item .env.example .env
php artisan key:generate
New-Item -ItemType File -Force database/database.sqlite
php artisan migrate
php artisan serve
```

If `.env` already exists, back it up before making changes; do not overwrite existing configuration unnecessarily.

Open `http://127.0.0.1:8000` in your browser.

For XAMPP, ensure `extension=pdo_sqlite` and `extension=sqlite3` are enabled in the `php.ini` used by your terminal. Restart the terminal and verify the extensions with:

```powershell
php -m
```

## First Use

1. Select **Create a workspace**. This creates a tenant and a manager account.
2. Add a nursery.
3. Create rooms and children under the relevant nursery.
4. The manager can create staff accounts and guardian accounts. Each guardian account must be linked to one child.
5. Staff can manage nurseries, rooms, children, and attendance, but cannot manage user accounts or delete managers.
6. Guardians can sign in to view only their linked child's current status and attendance history. Guardian accounts are read-only.

There are no default manager credentials in a newly initialized database. Operational data is entered through the application and stored in SQLite.

## Security and Business Rules

- All data queries and mutations are scoped to the signed-in user's tenant. Browser-supplied tenant IDs are not trusted.
- A tenant can contain multiple nurseries, with rooms and children associated with a nursery.
- A child can only check into a room belonging to their assigned nursery.
- A child can have only one active check-in at a time.
- Guardians can access attendance information only for their linked child.
- Only managers can create and delete user accounts.
- Managers cannot delete their own accounts or another manager through the UI.
- Attendance history is retained. Rooms and children with attendance history cannot be deleted.
- Children linked to guardian accounts cannot be deleted.
- Nurseries containing rooms or children cannot be deleted.
- The Laravel JSON API remains available under `/api` for integrations.
- The main user interface uses Blade and does not require Node.js or a separate frontend development server.

## Tests

Run the automated test suite from the Laravel project root:

```powershell
php artisan test
```

## Database Reset Warning

To intentionally start with an empty local database:

1. Stop the development server.
2. Back up `database/database.sqlite`.
3. Remove the existing database file only if you are certain its records are no longer needed.
4. Recreate the file.
5. Run `php artisan migrate`.

**Warning:** Removing the database file permanently deletes the records stored in that file. Never perform this operation on a database containing data you need.

## Existing User Credentials

The following credentials were supplied for an existing application environment. They will work only if the corresponding accounts exist in the database and the application accepts these credentials.

**Manager**
- Email: `admin@gmail.com`
- Password: `Admin@123456`

**Staff**
- Email: `staff@gmail.com`
- Password: `Staff@123456`

**Parent 1**
- Email: `parent1@gmail.com`
- Password: `Parents1@123456`

**Parent 2**
- Email: `parent2@gmail.com`
- Password: `Parents2@123456`

**Security note:** These are development or demonstration credentials. Do not use these passwords in production. Change them if the accounts are real, and never commit real credentials or secrets to a public repository.

---

## Project Goal

Nest Care provides a centralized system for managing nursery operations, rooms, children, attendance, staff, and guardian access. Its primary focus is data isolation between tenants, appropriate role-based permissions, reliable attendance workflows, and a straightforward interface for managers, staff, and parents.
