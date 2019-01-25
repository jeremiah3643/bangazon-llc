# The Nuclear Option: Or How I Stopped Worrying and Learned to Love Blowing Up My Database

As you work through the Bangazon assignments you will quickly discover that SQLite doesn't handle migrations all that well. Inevitably, if you create more than one migration file it will choke.

The best solution for now, to keep you moving forward and not losing precious sprint days, is to blow up your db and migration folder every time you need to change your models/db schema. It's easy enough to manually delete those, but if you've created data for that db, it goes bye-bye.

The following is an automated way of nuking your migrations and db, recreating your migration file and db, and repopulating (most*) tables. It consists of a shell script and a file to add to any Django project where you will be using SQLite and migrations:

1. ## The custom manage.py command:

In your Django app directory (not the project directory), create a `management` directory. Inside that directory create an empty `__init__.py` file and a `commands` directory. Then inside `commands` create an empty `__init__.py` file and a file called `seeder.py`. Use the code below as a guide to what to put into `seeder.py`. Note that the class of `Command` and its method `handle` are required names for this to work.

You'll need to `pip install django_seed`, a third-party module that does the heavy lifting of creating fake data to populate your tables.

Review django_seed's docs for the types of data it's capable of generating for you, then customise the `handle` method to suit your project's models.

```python
from django.core.management.base import BaseCommand
from django_seed import Seed
seeder = Seed.seeder()
import random
from foo.models import *

class Command(BaseCommand):

  # Examples of custom word lists you can instruct Django_Seed to use
  adjectives = [
  "Absorbing", "Adorable", "Adventurous", "Appealing",
  "Artistic", "Athletic", "Attractive", "Bold"]

  things = [
  "Alarm clock", "Armoire", "Backpack", "Bedding", "Bedspread", "Blankets", "Blinds", "Bookcase", "Books", "Broom", "Brush", "Bucket", "Calendar", "Candles", "Carpet", "Chair", "Chairs", "China", "Clock", "Coffee table", "Comb", "Comforter", "Computer"]

  def handle(self, *args, **options):
    # In the first two examples Django_Seed will use the Models' attributes (name, age, label, etc) to guess the right type of fake data to create
    seeder.add_entity(ProductType, 12) # the number argument is the total num of rows you want created
    seeder.add_entity(PaymentCategory, 5)

    # Or, you can alternately tell it exactly what kind tof data to use. Here we ask it to make arandom number.
    seeder.add_entity(PaymentType, 50, {'acct_number': lambda x: random.randint(11111,99999)})

    # Here, we make use of one of the built-in 'providers' (fake data type) that Django_Seed gives us
    # We are only customising the 'location' attribute. The seeder will automagically insert foreign keys if needed, and populate any other attributes by guessing what is best to insert
    seeder.add_entity(Customer, 13, {
      "location": lambda x: seeder.faker.city(),
    })

    # You can even combine faker providers to make a combo that suits your data needs best. These use the word lists defined above
    "title": lambda x: f"{seeder.faker.word(ext_word_list=adjectives)} {seeder.faker.word(ext_word_list=things)}",

    seeder.execute()
```

2. ## The shell script
You _can_ manually delete the db stuff, then directly run the custom command file above, but where's the fun in that? Saving this script where you can run everything with one command is heaps more bad-ass.
>For Mac users, save this file as `django_data.sh` in `usr/local/bin` (Which is at your machine's root, not your Users folder)

>For non-Mac users, who don't have a usr/local/bin, or if you're not comforatble setting up a global shell script command try just saving it locally at the project root and running it with `./django_data.sh <arg1> <arg2>`

```bash
#!/bin/bash

find ./$1/migrations/ -type f -name "*.py" -delete; #deletes all of the .py files in the migrations directory except for the __init__.py file.
find ./$1/migrations/ -type f -name "*.pyc" -delete; #deletes all of the .pyc files in the migrations directory.
rm db.sqlite3; #deletes the database file.
python manage.py makemigrations $1; #creates the migration.
python manage.py migrate; #runs the migration.
python manage.py $2 #runs the file we created above to seed the new db
```

You will need to change the permissions of this file with `chmod +x django_data.sh` to make it executable.
https://www.lifewire.com/uses-of-command-chmod-2201064


Now you can run it with *two args* <-- important:
1. The name of the django app that contains the models you want to add to the db schema/update
2. The name of the custom command file. If my app was called bangazon_api, then I would type

    > `django_data.sh bangazon_api faker_factory`


*NOTE: This process will only work properly for one-to-many relationships or Models with no foreign keys. For many-to-many, it's not tested. It _may_ work if you've defined a Class for your join tables. But, I doubt it. But, hey, prove me wrong!

*NOTE 2: This is tested on exacly one machine, so consider it experimental :)
If it doesn't work, come see an instructor and we'll see if we can make it run properly together.
