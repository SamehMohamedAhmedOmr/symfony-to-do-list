# Symfony To-Do List Project Documentation

## 📋 Project Overview

This is a **Symfony 7.2** based To-Do List application with event management capabilities. The project includes both web interfaces (using Twig templates) and REST API endpoints for task and event management. It features user authentication, JWT-based API authentication, and a complete CRUD system for tasks and events.

---

## 🏗️ Project Architecture

### Technology Stack

- **Framework**: Symfony 7.2
- **PHP Version**: >= 8.2
- **Database**: MySQL 8.0.32
- **ORM**: Doctrine ORM 3.3
- **Authentication**:
  - Web: Symfony Security Component with Form Login
  - API: Lexik JWT Authentication Bundle
- **Template Engine**: Twig
- **Testing**: PHPUnit 9.5
- **Additional Features**:
  - Pagination (KnpPaginatorBundle)
  - CORS Support (NelmioCorsBundle)
  - Asset Management (Symfony Asset Mapper)
  - Stimulus & Turbo for frontend interactivity

---

## 📁 Project Structure

```
symfony-to-do-list/
├── assets/              # Frontend assets (CSS, JS)
├── bin/                 # Executables (console)
├── config/              # Configuration files
│   ├── packages/        # Bundle configurations
│   ├── routes.yaml      # Route definitions
│   └── services.yaml    # Service definitions
├── migrations/          # Database migrations
├── public/             # Web root directory
│   └── index.php       # Front controller
├── src/                # Application source code
│   ├── Controller/     # Controllers (Web & API)
│   ├── Entity/         # Doctrine entities
│   ├── Repository/     # Database repositories
│   ├── Form/           # Form types
│   ├── DTO/            # Data Transfer Objects
│   ├── Security/       # Security components
│   └── Service/        # Business logic services
├── templates/          # Twig templates
├── tests/              # Unit and functional tests
├── translations/       # Translation files
├── var/                # Cache, logs, etc.
├── vendor/             # Composer dependencies
├── .env                # Environment variables
└── composer.json       # Dependencies and configuration
```

---

## 🗄️ Database Schema

### Entities

#### 1. **UserModal** (Users Table)

- Handles user authentication and management
- Repository: `UserModalRepository`

#### 2. **Events**

```php
- id: int (Primary Key, Auto-generated)
- name: string(255)
- event_date: DateTime (nullable)
- event_time: Time (nullable)
```

**Purpose**: Store calendar events that can be associated with tasks

#### 3. **TasksModel** (Tasks Table)

```php
- id: int (Primary Key, Auto-generated)
- user_id: int
- title: string(255)
- description: string(255)
- due_date: DateTime
- status: int (0=new, 1=in-progress, 2=completed)
- priority: int
- event_id: int (Foreign Key to Events)
- event: ManyToOne relationship with Events entity
```

**Purpose**: Store tasks with their associated details and relationships

#### 4. **SubTasksModel**

- Represents sub-tasks within main tasks
- Repository: `SubTasksModelRepository`

---

## 🎯 Core Features

### 1. **User Management**

- **Registration**: Users can register via `RegistrationController`
- **Authentication**:
  - Web login via `SecurityController` and `LoginFormAuthenticator`
  - API authentication via JWT tokens
- **Security Service**: `SecurityService` handles security-related operations

### 2. **Task Management (Web Interface)**

#### Routes:

- `GET /to-do-list` - List all tasks with pagination (10 items per page)
- `GET /to-do-list/create` - Display task creation form
- `POST /to-do-list/create` - Create a new task
- `GET /to-do-list/edit/{id}` - Display task edit form
- `POST /to-do-list/edit/{id}` - Update an existing task
- `GET /to-do-list/toggle/{id}` - Toggle task status (complete/incomplete)
- `GET /to-do-list/delete/{id}` - Delete a task

#### Controller: `ToDoListController`

- **Authorization**: All routes require `ROLE_USER`
- **Forms**: Uses `TaskType` form for creation and editing
- **Pagination**: Integrated with KnpPaginatorBundle

### 3. **Task Management (REST API)**

#### Controller: `TasksApiController`

- Provides RESTful API endpoints for tasks
- Uses `TaskTypeForAPI` form and `TaskDTO` for data transfer
- JWT authentication required

### 4. **Event Management**

#### Web Controller: `EventsController`

- CRUD operations for events
- Uses `EventType` form

#### API Controller: `EventsApiController`

- RESTful API for events
- Uses `EventDTO` for data transfer

### 5. **Home Page**

- Controller: `HomeController`
- Provides landing page and dashboard

---

## 🔐 Security Configuration

### Web Authentication

- **Login Authenticator**: `LoginFormAuthenticator`
- **Form-based login** with session management
- **Role-based access control**: `ROLE_USER` required for most features

### API Authentication

- **JWT Tokens** via Lexik JWT Authentication Bundle
- **Keys Location**:
  - Private key: `config/jwt/private.pem`
  - Public key: `config/jwt/public.pem`
- **Passphrase**: Configured in `.env` file

### CORS Configuration

- Allows requests from localhost and 127.0.0.1 with any port
- Configured via NelmioCorsBundle

---

## 🎨 Frontend Architecture

### Templates (Twig)

Located in `templates/` directory:

- `to_do_list/` - Task-related templates
  - `index.html.twig` - Task listing page
  - `create.html.twig` - Task creation form
  - `edit.html.twig` - Task editing form
- Additional template directories for other features

### Assets

- **Asset Mapper**: Modern asset management without Node.js
- **Stimulus**: JavaScript framework for sprinkles of interactivity
- **Turbo**: Makes navigation faster without full page reloads

---

## 📦 Key Bundles & Libraries

### Core Bundles

- `symfony/framework-bundle` - The core framework
- `doctrine/orm` - Database abstraction and ORM
- `symfony/security-bundle` - Authentication and authorization
- `symfony/twig-bundle` - Template engine integration

### Feature Bundles

- `lexik/jwt-authentication-bundle` - JWT authentication for API
- `knplabs/knp-paginator-bundle` - Database query pagination
- `nelmio/cors-bundle` - Cross-Origin Resource Sharing
- `symfony/maker-bundle` - Code generation tools (dev only)

### Development Tools

- `symfony/debug-bundle` - Debugging tools
- `symfony/web-profiler-bundle` - Profiler toolbar
- `phpunit/phpunit` - Unit testing framework

---

## 🗃️ Data Transfer Objects (DTOs)

### Purpose

DTOs are used to transfer data between layers, especially for API responses.

- **EventDTO**: Handles event data transformation
- **TaskDTO**: Handles task data transformation

---

## 🔄 Database Migrations

Located in `migrations/` directory. Migrations track database schema changes:

- `Version20250516084330.php`
- `Version20250516093439.php`
- `Version20250516094011.php`
- `Version20250521075327.php`
- `Version20250521075921.php`

### Running Migrations

```bash
php bin/console doctrine:migrations:migrate
```

---

## 🧪 Testing

### Test Structure

- **Location**: `tests/` directory
- **Framework**: PHPUnit with Symfony PHPUnit Bridge
- **Configuration**: `phpunit.xml.dist`

### Running Tests

```bash
php bin/phpunit
```

---

## ⚙️ Configuration Files

### Environment Variables (`.env`)

```env
APP_ENV=dev
APP_SECRET=[your-secret]
DATABASE_URL=mysql://root:@127.0.0.1:3306/to-do-list
JWT_PASSPHRASE=sameh
MESSENGER_TRANSPORT_DSN=doctrine://default?auto_setup=0
```

### Important Configurations

#### Doctrine (`config/packages/doctrine.yaml`)

- Auto-mapping enabled for entities
- Entity directory: `src/Entity`
- Underscore naming strategy

#### Routing (`config/routes.yaml`)

- Attribute-based routing
- Controllers auto-discovered from `src/Controller/`

---

## 🚀 Development Workflow

### Starting the Development Server

```bash
symfony server:start
# or
php -S localhost:8000 -t public/
```

### Clearing Cache

```bash
php bin/console cache:clear
```

### Database Commands

```bash
# Create database
php bin/console doctrine:database:create

# Run migrations
php bin/console doctrine:migrations:migrate

# Update schema (for development)
php bin/console doctrine:schema:update --force
```

### Generating Code

```bash
# Create a new entity
php bin/console make:entity

# Create a new controller
php bin/console make:controller

# Create a new form
php bin/console make:form

# Create a migration
php bin/console make:migration
```

---

## 📝 Form Types

Located in `src/Form/`:

1. **TaskType**: Form for task creation/editing (web interface)
2. **TaskTypeForAPI**: Form specifically for API endpoints
3. **EventType**: Form for event creation/editing
4. **RegistrationFormTypeForm**: User registration form

---

## 🔍 Repository Pattern

Each entity has a corresponding repository in `src/Repository/`:

- **EventsRepository**: Custom queries for events
- **TasksModelRepository**: Custom queries for tasks (includes QueryBuilder for pagination)
- **SubTasksModelRepository**: Custom queries for sub-tasks
- **UserModalRepository**: User-related queries

---

## 🌍 Internationalization

### Translation System

- **Location**: `translations/` directory
- **Symfony Translation Component** enabled
- Translations can be used in templates and PHP code

---

## 📊 Status Codes

### Task Status

- `0` - New/Pending
- `1` - In Progress
- `2` - Completed

The `toggle` action switches between status `1`/`0` and status `2`.

---

## 🔗 API Endpoints Overview

### Authentication

- `POST /api/login` - Obtain JWT token
- `POST /api/register` - Register new user

### Tasks API

- `GET /api/tasks` - List all tasks
- `POST /api/tasks` - Create a new task
- `GET /api/tasks/{id}` - Get a specific task
- `PUT /api/tasks/{id}` - Update a task
- `DELETE /api/tasks/{id}` - Delete a task

### Events API

- `GET /api/events` - List all events
- `POST /api/events` - Create a new event
- `GET /api/events/{id}` - Get a specific event
- `PUT /api/events/{id}` - Update an event
- `DELETE /api/events/{id}` - Delete an event

_(Note: Exact route patterns may vary - check controllers for precise URLs)_

---

## 🛠️ Development Tools

### Symfony Console

```bash
# List all available commands
php bin/console list

# Get help for a specific command
php bin/console help [command]

# Debug routes
php bin/console debug:router

# Debug services
php bin/console debug:container

# Debug configuration
php bin/console debug:config
```

### Web Profiler

Available in dev environment at `/_profiler`

- Performance metrics
- Database queries
- Form data
- Security information
- And much more!

---

## 📦 Composer Scripts

Defined in `composer.json`:

```json
"auto-scripts": {
    "cache:clear": "symfony-cmd",
    "assets:install %PUBLIC_DIR%": "symfony-cmd",
    "importmap:install": "symfony-cmd"
}
```

These run automatically after:

- `composer install`
- `composer update`

---

## 🔒 Best Practices Implemented

1. **Dependency Injection**: Controllers use constructor injection
2. **Repository Pattern**: Database queries isolated in repositories
3. **Form Validation**: Form types handle validation
4. **Security**: Role-based access control with `#[IsGranted]` attributes
5. **Separation of Concerns**:
   - Controllers handle HTTP
   - Entities represent data
   - Repositories handle queries
   - Services contain business logic
6. **Environment Configuration**: Sensitive data in `.env` files
7. **Migration-based Schema Management**: All schema changes tracked

---

## 🐛 Debugging Tips

### Enable Debug Mode

Ensure `APP_ENV=dev` in `.env` file

### Common Debug Commands

```bash
# Check if routes are registered
php bin/console debug:router

# Validate Doctrine schema
php bin/console doctrine:schema:validate

# Check autowiring
php bin/console debug:autowiring

# View all services
php bin/console debug:container
```

### Logs

Located in `var/log/`:

- `dev.log` - Development logs
- `prod.log` - Production logs
- `test.log` - Test logs

---

## 🚀 Deployment Considerations

### Production Checklist

1. Set `APP_ENV=prod` in `.env`
2. Generate JWT keys if not present
3. Run migrations: `php bin/console doctrine:migrations:migrate`
4. Clear cache: `php bin/console cache:clear --env=prod`
5. Install assets: `php bin/console assets:install --env=prod`
6. Set proper file permissions on `var/` directory
7. Configure a proper web server (Apache/Nginx)
8. Use environment variables for sensitive data
9. Enable OPcache for PHP
10. Configure CORS for production domains

---

## 📚 Additional Resources

- [Symfony Documentation](https://symfony.com/doc/current/index.html)
- [Doctrine ORM Documentation](https://www.doctrine-project.org/projects/orm.html)
- [Twig Documentation](https://twig.symfony.com/)
- [Lexik JWT Authentication](https://github.com/lexik/LexikJWTAuthenticationBundle)

---

## 📞 Support & Contribution

This project demonstrates a complete Symfony application with:

- ✅ User authentication (Web & API)
- ✅ CRUD operations
- ✅ Form handling
- ✅ Database relationships
- ✅ REST API
- ✅ Security best practices
- ✅ Testing setup

Perfect for learning Symfony or as a foundation for more complex applications!
