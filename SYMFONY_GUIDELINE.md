# Complete Symfony Development Guideline

## 📖 Table of Contents

1. [Introduction to Symfony](#introduction-to-symfony)
2. [System Requirements](#system-requirements)
3. [Installation & Setup](#installation--setup)
4. [Creating a New Symfony Project](#creating-a-new-symfony-project)
5. [Project Structure](#project-structure)
6. [Essential Symfony Commands](#essential-symfony-commands)
7. [Working with Doctrine ORM](#working-with-doctrine-orm)
8. [Controllers & Routing](#controllers--routing)
9. [Templates with Twig](#templates-with-twig)
10. [Forms](#forms)
11. [Security & Authentication](#security--authentication)
12. [REST API Development](#rest-api-development)
13. [Testing](#testing)
14. [Best Practices](#best-practices)
15. [Troubleshooting](#troubleshooting)

---

## 🌟 Introduction to Symfony

**Symfony** is a powerful PHP framework for web applications and a set of reusable PHP components. It follows the **Model-View-Controller (MVC)** architectural pattern and emphasizes:

- **Reusable components**
- **Best practices**
- **Standardization**
- **Flexibility**
- **Performance**

### Why Choose Symfony?

✅ **Enterprise-ready** - Used by major companies worldwide  
✅ **Long-term support** - LTS versions supported for 4 years  
✅ **Rich ecosystem** - Thousands of bundles and components  
✅ **Great documentation** - Comprehensive and up-to-date  
✅ **Active community** - Large, helpful developer community  
✅ **Modern PHP** - Leverages latest PHP features

---

## ⚙️ System Requirements

### Minimum Requirements

- **PHP**: 8.2 or higher
- **Composer**: Latest version
- **Extensions**:
  - `ctype`
  - `iconv`
  - `json`
  - `pdo`
  - `session`
  - `tokenizer`
  - `xml`

### Recommended

- **Symfony CLI**: For easier project management
- **Database**: MySQL, PostgreSQL, or SQLite
- **Web Server**: Apache, Nginx, or Symfony's built-in server
- **Git**: For version control

---

## 🚀 Installation & Setup

### Step 1: Install PHP

**Windows (with Laragon/XAMPP):**

- Download and install [Laragon](https://laragon.org/) or [XAMPP](https://www.apachefriends.org/)
- PHP is included

**macOS:**

```bash
brew install php@8.2
```

**Linux (Ubuntu/Debian):**

```bash
sudo apt update
sudo apt install php8.2 php8.2-cli php8.2-common php8.2-mysql php8.2-xml php8.2-curl
```

### Step 2: Install Composer

Visit [getcomposer.org](https://getcomposer.org/) and follow installation instructions, or:

**Linux/macOS:**

```bash
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
```

**Windows:**
Download and run the [Composer-Setup.exe](https://getcomposer.org/Composer-Setup.exe)

### Step 3: Install Symfony CLI (Optional but Recommended)

**Linux/macOS:**

```bash
curl -sS https://get.symfony.com/cli/installer | bash
```

**Windows:**
Download from [symfony.com/download](https://symfony.com/download)

**Verify installation:**

```bash
symfony version
```

---

## 🆕 Creating a New Symfony Project

### Method 1: Using Symfony CLI (Recommended)

#### Full-Stack Web Application

```bash
symfony new my-project --webapp
cd my-project
```

#### Microservice or API Application

```bash
symfony new my-project
cd my-project
```

#### Specific Symfony Version

```bash
symfony new my-project --version=7.2
```

### Method 2: Using Composer

#### Full-Stack Web Application

```bash
composer create-project symfony/skeleton my-project
cd my-project
composer require webapp
```

#### Microservice/API

```bash
composer create-project symfony/skeleton:"7.2.*" my-project
cd my-project
```

---

## 📁 Project Structure

```
my-symfony-project/
│
├── assets/              # Frontend assets (CSS, JS)
├── bin/
│   └── console          # Symfony console application
├── config/
│   ├── packages/        # Bundle configurations
│   ├── routes.yaml      # Application routes
│   └── services.yaml    # Service container configuration
├── migrations/          # Database migrations
├── public/
│   └── index.php        # Front controller (entry point)
├── src/
│   ├── Controller/      # Controllers
│   ├── Entity/          # Doctrine entities
│   ├── Repository/      # Doctrine repositories
│   ├── Form/            # Form types
│   ├── Security/        # Security related code
│   └── Kernel.php       # Application kernel
├── templates/           # Twig templates
├── tests/               # Automated tests
├── translations/        # Translation files
├── var/
│   ├── cache/          # Application cache
│   └── log/            # Application logs
├── vendor/             # Composer dependencies (git-ignored)
├── .env                # Environment variables
├── .env.local          # Local environment overrides (git-ignored)
├── composer.json       # PHP dependencies
└── symfony.lock        # Package versions lock
```

---

## 🎯 Essential Symfony Commands

### Console Commands

#### General Commands

```bash
# List all available commands
php bin/console list

# Get help for a specific command
php bin/console help doctrine:migrations:migrate

# Clear cache
php bin/console cache:clear

# Clear cache for production
php bin/console cache:clear --env=prod

# Check Symfony requirements
symfony check:requirements
```

### Development Server

```bash
# Start development server (Symfony CLI)
symfony server:start

# Start in background
symfony server:start -d

# Stop server
symfony server:stop

# Check server status
symfony server:status

# Alternative: PHP built-in server
php -S localhost:8000 -t public/
```

### Code Generation (Maker Bundle)

```bash
# Install Maker Bundle (if not already installed)
composer require --dev symfony/maker-bundle

# Create a new controller
php bin/console make:controller ProductController

# Create a new entity
php bin/console make:entity Product

# Create a new form type
php bin/console make:form ProductType

# Create a CRUD controller
php bin/console make:crud Product

# Create a voter (for authorization)
php bin/console make:voter

# Create a command
php bin/console make:command app:my-command

# Create authentication
php bin/console make:auth

# Create registration form
php bin/console make:registration-form

# Create a subscriber
php bin/console make:subscriber

# Create a validator
php bin/console make:validator

# Create a migration
php bin/console make:migration
```

### Debugging Commands

```bash
# List all routes
php bin/console debug:router

# Show details for a specific route
php bin/console debug:router app_product_show

# List all services
php bin/console debug:container

# Debug autowiring
php bin/console debug:autowiring

# Debug configuration for a bundle
php bin/console debug:config framework

# View all event listeners
php bin/console debug:event-dispatcher
```

---

## 🗄️ Working with Doctrine ORM

### Installation

```bash
# Install Doctrine
composer require symfony/orm-pack
composer require --dev symfony/maker-bundle
```

### Database Configuration

Edit `.env` file:

```env
# For MySQL
DATABASE_URL="mysql://username:password@127.0.0.1:3306/database_name?serverVersion=8.0.32&charset=utf8mb4"

# For PostgreSQL
DATABASE_URL="postgresql://username:password@127.0.0.1:5432/database_name?serverVersion=15&charset=utf8"

# For SQLite
DATABASE_URL="sqlite:///%kernel.project_dir%/var/data.db"
```

### Database Commands

```bash
# Create database
php bin/console doctrine:database:create

# Drop database
php bin/console doctrine:database:drop --force

# Create entity
php bin/console make:entity Product

# Update entity (add fields)
php bin/console make:entity Product

# Generate migration
php bin/console make:migration

# Execute migrations
php bin/console doctrine:migrations:migrate

# Rollback last migration
php bin/console doctrine:migrations:migrate prev

# Check migration status
php bin/console doctrine:migrations:status

# Validate schema (for development)
php bin/console doctrine:schema:validate

# Update schema directly (NOT for production!)
php bin/console doctrine:schema:update --force
```

### Creating an Entity

```bash
php bin/console make:entity Product
```

This creates `src/Entity/Product.php`:

```php
<?php

namespace App\Entity;

use App\Repository\ProductRepository;
use Doctrine\ORM\Mapping as ORM;

#[ORM\Entity(repositoryClass: ProductRepository::class)]
class Product
{
    #[ORM\Id]
    #[ORM\GeneratedValue]
    #[ORM\Column]
    private ?int $id = null;

    #[ORM\Column(length: 255)]
    private ?string $name = null;

    #[ORM\Column]
    private ?float $price = null;

    // Getters and setters...
}
```

### Working with Entities

```php
// In a controller
use App\Entity\Product;
use Doctrine\ORM\EntityManagerInterface;

public function create(EntityManagerInterface $em): Response
{
    $product = new Product();
    $product->setName('Laptop');
    $product->setPrice(999.99);

    // Persist to database
    $em->persist($product);
    $em->flush();

    return new JsonResponse(['id' => $product->getId()]);
}

public function update(EntityManagerInterface $em, int $id): Response
{
    $product = $em->getRepository(Product::class)->find($id);

    if (!$product) {
        throw $this->createNotFoundException();
    }

    $product->setPrice(899.99);
    $em->flush(); // No need to persist() for existing entities

    return new JsonResponse(['success' => true]);
}

public function delete(EntityManagerInterface $em, int $id): Response
{
    $product = $em->getRepository(Product::class)->find($id);

    if (!$product) {
        throw $this->createNotFoundException();
    }

    $em->remove($product);
    $em->flush();

    return new JsonResponse(['success' => true]);
}
```

### Repository Queries

```php
// In ProductRepository.php
public function findExpensive(): array
{
    return $this->createQueryBuilder('p')
        ->andWhere('p.price > :price')
        ->setParameter('price', 500)
        ->orderBy('p.price', 'DESC')
        ->getQuery()
        ->getResult();
}

public function findByNameLike(string $name): array
{
    return $this->createQueryBuilder('p')
        ->andWhere('p.name LIKE :name')
        ->setParameter('name', '%' . $name . '%')
        ->getQuery()
        ->getResult();
}
```

---

## 🎮 Controllers & Routing

### Creating a Controller

```bash
php bin/console make:controller ProductController
```

### Controller Example

```php
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\Routing\Attribute\Route;

#[Route('/product')]
class ProductController extends AbstractController
{
    #[Route('/', name: 'product_index', methods: ['GET'])]
    public function index(): Response
    {
        return $this->render('product/index.html.twig', [
            'products' => [],
        ]);
    }

    #[Route('/new', name: 'product_new', methods: ['GET', 'POST'])]
    public function new(Request $request): Response
    {
        // Handle form submission
        return $this->render('product/new.html.twig');
    }

    #[Route('/{id}', name: 'product_show', methods: ['GET'])]
    public function show(int $id): Response
    {
        return $this->render('product/show.html.twig', [
            'id' => $id,
        ]);
    }

    #[Route('/{id}/edit', name: 'product_edit', methods: ['GET', 'POST'])]
    public function edit(Request $request, int $id): Response
    {
        return $this->render('product/edit.html.twig');
    }

    #[Route('/{id}', name: 'product_delete', methods: ['DELETE'])]
    public function delete(int $id): Response
    {
        return $this->redirectToRoute('product_index');
    }
}
```

### Routing Annotations

```php
#[Route('/path', name: 'route_name', methods: ['GET', 'POST'])]

// Route with parameters
#[Route('/product/{id}', name: 'product_show', requirements: ['id' => '\d+'])]

// Optional parameters
#[Route('/product/{id?}', name: 'product_show')]

// Parameter constraints
#[Route('/product/{id}', requirements: ['id' => '\d+'])]
```

### Route Debugging

```bash
# List all routes
php bin/console debug:router

# Filter routes by name
php bin/console debug:router | grep product

# Show route details
php bin/console debug:router product_show
```

---

## 🎨 Templates with Twig

### Template Location

Templates are stored in `templates/` directory.

### Basic Template Syntax

```twig
{# templates/product/index.html.twig #}
{% extends 'base.html.twig' %}

{% block title %}Product List{% endblock %}

{% block body %}
    <h1>Products</h1>

    {% if products is empty %}
        <p>No products found.</p>
    {% else %}
        <ul>
            {% for product in products %}
                <li>
                    <a href="{{ path('product_show', {'id': product.id}) }}">
                        {{ product.name }} - ${{ product.price }}
                    </a>
                </li>
            {% endfor %}
        </ul>
    {% endif %}

    <a href="{{ path('product_new') }}" class="btn">Create New</a>
{% endblock %}
```

### Common Twig Functions

```twig
{# Generate URL #}
{{ path('route_name') }}
{{ path('product_show', {'id': 5}) }}
{{ url('route_name') }} {# Absolute URL #}

{# Asset paths #}
{{ asset('images/logo.png') }}
{{ asset('css/app.css') }}

{# Translation #}
{{ 'hello.world'|trans }}

{# Date formatting #}
{{ product.createdAt|date('Y-m-d') }}

{# Escape HTML #}
{{ product.description|raw }}

{# Dump variable (debug) #}
{{ dump(product) }}
```

---

## 📝 Forms

### Creating a Form Type

```bash
php bin/console make:form ProductType
```

This creates `src/Form/ProductType.php`:

```php
<?php

namespace App\Form;

use App\Entity\Product;
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\Extension\Core\Type\MoneyType;
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\Extension\Core\Type\SubmitType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\OptionsResolver\OptionsResolver;

class ProductType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options): void
    {
        $builder
            ->add('name', TextType::class, [
                'label' => 'Product Name',
                'required' => true,
            ])
            ->add('price', MoneyType::class, [
                'label' => 'Price',
                'currency' => 'USD',
            ])
            ->add('save', SubmitType::class, [
                'label' => 'Create Product',
            ]);
    }

    public function configureOptions(OptionsResolver $resolver): void
    {
        $resolver->setDefaults([
            'data_class' => Product::class,
        ]);
    }
}
```

### Using Forms in Controllers

```php
use App\Entity\Product;
use App\Form\ProductType;
use Symfony\Component\HttpFoundation\Request;

#[Route('/product/new', name: 'product_new')]
public function new(Request $request, EntityManagerInterface $em): Response
{
    $product = new Product();
    $form = $this->createForm(ProductType::class, $product);

    $form->handleRequest($request);

    if ($form->isSubmitted() && $form->isValid()) {
        $em->persist($product);
        $em->flush();

        $this->addFlash('success', 'Product created successfully!');

        return $this->redirectToRoute('product_index');
    }

    return $this->render('product/new.html.twig', [
        'form' => $form->createView(),
    ]);
}
```

### Rendering Forms in Twig

```twig
{# templates/product/new.html.twig #}
{% extends 'base.html.twig' %}

{% block body %}
    <h1>Create Product</h1>

    {{ form_start(form) }}
        {{ form_widget(form) }}
    {{ form_end(form) }}

    {# Or render fields individually #}
    {{ form_start(form) }}
        <div>
            {{ form_label(form.name) }}
            {{ form_widget(form.name) }}
            {{ form_errors(form.name) }}
        </div>

        <div>
            {{ form_label(form.price) }}
            {{ form_widget(form.price) }}
            {{ form_errors(form.price) }}
        </div>

        <button type="submit">Create</button>
    {{ form_end(form) }}
{% endblock %}
```

---

## 🔒 Security & Authentication

### Install Security Component

```bash
composer require symfony/security-bundle
```

### Create User Entity

```bash
php bin/console make:user
```

### Create Authentication

```bash
# Create login form authenticator
php bin/console make:auth

# Create registration form
php bin/console make:registration-form
```

### Security Configuration

Edit `config/packages/security.yaml`:

```yaml
security:
  password_hashers:
    Symfony\Component\Security\Core\User\PasswordAuthenticatedUserInterface: "auto"

  providers:
    app_user_provider:
      entity:
        class: App\Entity\User
        property: email

  firewalls:
    dev:
      pattern: ^/(_(profiler|wdt)|css|images|js)/
      security: false

    main:
      lazy: true
      provider: app_user_provider
      form_login:
        login_path: app_login
        check_path: app_login
      logout:
        path: app_logout

  access_control:
    - { path: ^/admin, roles: ROLE_ADMIN }
    - { path: ^/profile, roles: ROLE_USER }
```

### Protecting Routes

```php
use Symfony\Component\Security\Http\Attribute\IsGranted;

#[Route('/admin')]
#[IsGranted('ROLE_ADMIN')]
class AdminController extends AbstractController
{
    // All actions require ROLE_ADMIN
}

// Or for specific actions
#[Route('/profile')]
#[IsGranted('ROLE_USER')]
public function profile(): Response
{
    // Requires ROLE_USER
}
```

### Getting Current User

```php
public function index(): Response
{
    $user = $this->getUser();

    if (!$user) {
        throw $this->createAccessDeniedException();
    }

    return $this->render('profile/index.html.twig', [
        'user' => $user,
    ]);
}
```

---

## 🔌 REST API Development

### Install API Platform (Optional)

```bash
composer require api
```

### Or Build Custom API

#### Install Required Packages

```bash
# For JWT authentication
composer require lexik/jwt-authentication-bundle

# For CORS
composer require nelmio/cors-bundle

# For serialization
composer require symfony/serializer
```

#### Generate JWT Keys

```bash
php bin/console lexik:jwt:generate-keypair
```

#### API Controller Example

```php
<?php

namespace App\Controller\API;

use App\Entity\Product;
use App\Repository\ProductRepository;
use Doctrine\ORM\EntityManagerInterface;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\JsonResponse;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\Routing\Attribute\Route;

#[Route('/api/products')]
class ProductApiController extends AbstractController
{
    #[Route('', name: 'api_product_index', methods: ['GET'])]
    public function index(ProductRepository $repository): JsonResponse
    {
        $products = $repository->findAll();

        return $this->json($products);
    }

    #[Route('/{id}', name: 'api_product_show', methods: ['GET'])]
    public function show(Product $product): JsonResponse
    {
        return $this->json($product);
    }

    #[Route('', name: 'api_product_create', methods: ['POST'])]
    public function create(Request $request, EntityManagerInterface $em): JsonResponse
    {
        $data = json_decode($request->getContent(), true);

        $product = new Product();
        $product->setName($data['name']);
        $product->setPrice($data['price']);

        $em->persist($product);
        $em->flush();

        return $this->json($product, 201);
    }

    #[Route('/{id}', name: 'api_product_update', methods: ['PUT'])]
    public function update(Request $request, Product $product, EntityManagerInterface $em): JsonResponse
    {
        $data = json_decode($request->getContent(), true);

        $product->setName($data['name'] ?? $product->getName());
        $product->setPrice($data['price'] ?? $product->getPrice());

        $em->flush();

        return $this->json($product);
    }

    #[Route('/{id}', name: 'api_product_delete', methods: ['DELETE'])]
    public function delete(Product $product, EntityManagerInterface $em): JsonResponse
    {
        $em->remove($product);
        $em->flush();

        return $this->json(['message' => 'Product deleted'], 204);
    }
}
```

---

## 🧪 Testing

### Install PHPUnit

```bash
composer require --dev symfony/phpunit-bridge
```

### Run Tests

```bash
# Run all tests
php bin/phpunit

# Run specific test file
php bin/phpunit tests/Controller/ProductControllerTest.php

# Run with coverage
php bin/phpunit --coverage-html var/coverage
```

### Create Test

```bash
php bin/console make:test

# Choose type:
# - TestCase (basic PHPUnit test)
# - KernelTestCase (has access to services)
# - WebTestCase (functional test with HTTP client)
```

### Example Functional Test

```php
<?php

namespace App\Tests\Controller;

use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class ProductControllerTest extends WebTestCase
{
    public function testIndex(): void
    {
        $client = static::createClient();
        $client->request('GET', '/product/');

        $this->assertResponseIsSuccessful();
        $this->assertSelectorTextContains('h1', 'Products');
    }

    public function testCreate(): void
    {
        $client = static::createClient();
        $crawler = $client->request('GET', '/product/new');

        $form = $crawler->selectButton('Create')->form([
            'product[name]' => 'Test Product',
            'product[price]' => '99.99',
        ]);

        $client->submit($form);

        $this->assertResponseRedirects('/product/');
    }
}
```

---

## 💡 Best Practices

### 1. Use Environment Variables

Store sensitive information in `.env` files, never commit `.env.local`

### 2. Follow Symfony Coding Standards

```bash
composer require --dev friendsofphp/php-cs-fixer
vendor/bin/php-cs-fixer fix src
```

### 3. Use Dependency Injection

```php
// Good: Constructor injection
public function __construct(
    private EntityManagerInterface $em,
    private ProductRepository $repository
) {}

// Avoid: Service locator pattern
$em = $this->getDoctrine()->getManager();
```

### 4. Validate Input

Use Symfony's validation constraints

### 5. Use Migrations

Never use `doctrine:schema:update` in production

### 6. Profile Your Application

Use the Symfony Profiler toolbar in development

### 7. Cache Configuration

Configure cache properly for production

### 8. Use Bundles Wisely

Only install bundles you actually need

---

## 🔧 Troubleshooting

### Clear Cache

```bash
php bin/console cache:clear
php bin/console cache:warmup
```

### Check Requirements

```bash
symfony check:requirements
```

### Fix Permissions (Linux/macOS)

```bash
chmod -R 777 var/
```

### Database Connection Issues

- Check `DATABASE_URL` in `.env`
- Verify database server is running
- Check credentials

### Composer Issues

```bash
composer clear-cache
composer update
```

### Debugging Routes

```bash
php bin/console debug:router
```

### Check Service Configuration

```bash
php bin/console debug:container
php bin/console debug:autowiring
```

---

## 📚 Additional Resources

### Official Documentation

- [Symfony Documentation](https://symfony.com/doc/current/index.html)
- [Symfony Best Practices](https://symfony.com/doc/current/best_practices.html)
- [Doctrine Documentation](https://www.doctrine-project.org/)

### Useful Commands Reference

```bash
# Project creation
symfony new my-project --webapp

# Development server
symfony server:start

# Code generation
php bin/console make:controller
php bin/console make:entity
php bin/console make:form
php bin/console make:crud

# Database
php bin/console doctrine:database:create
php bin/console make:migration
php bin/console doctrine:migrations:migrate

# Debugging
php bin/console debug:router
php bin/console debug:container
php bin/console debug:autowiring

# Cache
php bin/console cache:clear
php bin/console cache:warmup

# Assets
php bin/console assets:install

# Testing
php bin/phpunit
```

---

## 🎓 Learning Path

1. **Start with basics**: Controllers, routing, templates
2. **Learn Doctrine**: Entities, repositories, relationships
3. **Master forms**: Form types, validation
4. **Understand security**: Authentication, authorization
5. **Build APIs**: RESTful endpoints, JWT
6. **Write tests**: Unit tests, functional tests
7. **Optimize**: Caching, performance
8. **Deploy**: Production configuration, deployment strategies

---

**Happy Coding with Symfony! 🚀**
