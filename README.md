# GGCode Python MySQL Migration Project Template

## Installation

pre requisite:
- Python 3.7
- Python Virtual Environment (we usually use this https://pypi.org/project/virtualenv/)
- Maria DB database

Install Command:
- Activate your Python Virtual Environment, if using virtualenv: 
```
<virtualenv_directory>/bin/activate**)
```
- Install Libraries:
```
pip install -r requirement.txt
```
- create mysql/mariadb database
- copy orator.yaml.example to orator.yaml
- change orator connection in orator.yaml
- run migration & seeder: 
```
orator migrate --seed -c orator.yaml
```

Uninstall Command:
- Remove migration: 
```
orator migrate:reset -c orator.yaml
```
- Remove libraries: 
```
pip uninstall -r requirement.txt
```

## Directory Structure

- migrations: location for migration file
- seeds: location for data seeder file


## How to Create or Alter Table

Learn how to modify database using orator-orm migration here https://orator-orm.com/docs/0.9/migrations.html

Data type list in migration file here https://orator-orm.com/docs/0.9/schema_builder.html#adding-columns

See the example [migrations/2018_08_04_053754_create_user_table.py](https://github.com/abcdef-id/python-mysql-migration/blob/master/migrations/2018_08_04_053754_create_user_table.py)

For this project template, below are **mandatory** fields, don't forget to put this fields in your migration file:
```
    table.integer('status').default(0)
    table.integer('created_by').nullable()
    table.integer('updated_by').nullable()
    table.timestamps()
```

### This is summary from the orator-orm.com

To create a migration, run:

```
orator make:migration create_users_table
```

This will create a migration file in migration directory that looks like this:

```
from orator.migrations import Migration


class CreateTableUsers(Migration):

    def up(self):
        """
        Run the migrations.
        """
        pass

    def down(self):
        """
        Revert the migrations.
        """
        pass
```

The --table and --create options can also be used to indicate the name of the table, and whether the migration will be creating a new table:

```
orator make:migration add_votes_to_users_table --table=users

orator make:migration create_users_table --table=users --create
```

These commands would respectively create the following migrations:

```
from orator.migrations import Migration


class AddVotesToUsersTable(Migration):

    def up(self):
        """
        Run the migrations.
        """
        with self.schema.table('users') as table:
            pass

    def down(self):
        """
        Revert the migrations.
        """
        with self.schema.table('users') as table:
            pass
```
```
from orator.migrations import Migration


class CreateTableUsers(Migration):

    def up(self):
        """
        Run the migrations.
        """
        with self.schema.create('users') as table:
            table.increments('id')
            table.timestamps()

    def down(self):
        """
        Revert the migrations.
        """
        self.schema.drop('users')
```

To run migration, run this command:

```
orator migrate -c orator.yaml
```

To see the status of the migrations, just use the migrate:status command:

```
orator migrate:status -c orator.yaml
```
```
+----------------------------------------------------+------+
| Migration                                          | Ran? |
+----------------------------------------------------+------+
| 2015_05_02_04371430559457_create_users_table       | Yes  |
| 2015_05_04_02361430725012_add_votes_to_users_table | No   |
+----------------------------------------------------+------+
```

## How to Create Data Seeder

Learn how to make data seeder here https://orator-orm.com/docs/0.9/seeding.html

### This is summary from the orator-orm.com

To generate a seeder:

```
orator make:seed user_table_seeder
```

Write your seeder, see the example [seeds/user_table_seeder.py](https://github.com/abcdef-id/python-mysql-migration/blob/master/seeds/user_table_seeder.py)

Import and call you seeder in [seeds/database_seeder.py](https://github.com/abcdef-id/python-mysql-migration/blob/master/seeds/database_seeder.py)



