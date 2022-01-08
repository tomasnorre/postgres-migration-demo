# PostgreSQL Migration Demo

## Problem

When running symfony and PostgreSQL when working with ORM, everytime when I run make migration it creates a new migration with following content

```php
public function down(Schema $schema): void
{
    // this down() migration is auto-generated, please modify it to your needs
    $this->addSql('CREATE SCHEMA public');
}
```

I don't get why it claims to always need to do that? The first migration created contained a that already 
```php 
public function down(Schema $schema): void
{
    // this down() migration is auto-generated, please modify it to your needs
    $this->addSql('CREATE SCHEMA public');
    $this->addSql('DROP SEQUENCE demo_id_seq CASCADE');
    $this->addSql('DROP TABLE demo');
}
```

To me this sounds like an unwanted behaviour, someone with more input on this, or that can confirms this, or perhaps explain to me if I'm doing something wrong. 


### Versions
Symfony 6.0.0
PHP 8.1
PostgreSQL 13-alpine

## Steps to reproduce 

```shell
$ composer create-project symfony/skeleton postgres-migration-demo
$ composer require symfony/maker-bundle --dev
$ composer require orm 
    # answer yes to 
    The recipe for this package contains some Docker configuration.
    This may create/update docker-compose.yml or update Dockerfile (if it exists).

    Do you want to include Docker configuration from recipes?
    [y] Yes
    [n] No
    [p] Yes permanently, never ask again for this project
    [x] No permanently, never ask again for this project
    (defaults to y):
     
$ docker-compose up -d
# Create on entity with at least one property 
$ bin/console make:entity 

$ tree migrations/ 
migrations/

0 directories, 0 files

# Create migration
$ bin/console make:migration
           
  Success! 

 Next: Review the new migration "migrations/Version20220108133248.php"
 Then: Run the migration with php bin/console doctrine:migrations:migrate
```

Content of `migrations/Version20220108133248.php`
```php 
...
/**
 * Auto-generated Migration: Please modify to your needs!
 */
final class Version20220108133248 extends AbstractMigration
{
    public function getDescription(): string
    {
        return '';
    }

    public function up(Schema $schema): void
    {
        // this up() migration is auto-generated, please modify it to your needs
        $this->addSql('CREATE SEQUENCE demo_id_seq INCREMENT BY 1 MINVALUE 1 START 1');
        $this->addSql('CREATE TABLE demo (id INT NOT NULL, title VARCHAR(255) NOT NULL, PRIMARY KEY(id))');
    }

    public function down(Schema $schema): void
    {
        // this down() migration is auto-generated, please modify it to your needs
        $this->addSql('CREATE SCHEMA public');
        $this->addSql('DROP SEQUENCE demo_id_seq CASCADE');
        $this->addSql('DROP TABLE demo');
    }
}
```

```shell
$ bin/console doctrine:migrations:migrate

 WARNING! You are about to execute a migration in database "app" that could result in schema changes and data loss. Are you sure you wish to continue? (yes/no) [yes]:
 > yes

[notice] Migrating up to DoctrineMigrations\Version20220108133248
[notice] finished in 46.5ms, used 14M memory, 1 migrations executed, 2 sql queries
```

```shell
$ bin/console make:migration              
          
  Success! 

 Next: Review the new migration "migrations/Version20220108133424.php"
 Then: Run the migration with php bin/console doctrine:migrations:migrate
```

Content of `migrations/Version20220108133424.php`
```php
... 
/**
 * Auto-generated Migration: Please modify to your needs!
 */
final class Version20220108133424 extends AbstractMigration
{
    public function getDescription(): string
    {
        return '';
    }

    public function up(Schema $schema): void
    {
        // this up() migration is auto-generated, please modify it to your needs

    }

    public function down(Schema $schema): void
    {
        // this down() migration is auto-generated, please modify it to your needs
        $this->addSql('CREATE SCHEMA public');
    }
}
```