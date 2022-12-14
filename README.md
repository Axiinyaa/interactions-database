# interactions-database
A simple json database writer for interactions-py that allows for simple persistence.

## Doesn't Persistence or wait-for exist?
Persistence, wait-for and Database all have their own use cases. For example, Persistence and wait-for does NOT write to disk, while Database does. This library isn't designed to compete with either of those libraries, and is more focused on storing data quickly to be used at any time.

## How do I use it?
First, install the extension using this command.
```
pip install git+https://github.com/Axiinyaa/interactions-database
```

Once installed, import the module like so.

```py
import interactions
from interactions.ext.database import Database

bot = interactions.Client(...)
```

Then, to create a database, use ``Database.create_database()``. It's highly recommended to do this during ``on_start()``.
```py
import interactions
from interactions.ext.database import Database

bot = interactions.Client(...)

@bot.event()
async def on_start()

    default_data = {"amount_of_coins" : 0}

    await Database.create_database(
            name = 'coins',
            type = Database.DatabaseType.USER,
            default_data = default_data
        )
```

To get data from the database, use ``Database.get_item()``, to set data, use ``Database.set_item()``.

```py

# Getting the Database called 'coins'
db = await Database.get_item(uid = ctx, database = 'coins')

# Grabbing a value from the database. This a dictionary so it's recommended to use the get() function.
coins = db.get('amount_of_coins', 0)

# Setting the amount_of_coins value in the 'coins' database.
await Database.set_item(uid = ctx, database = 'coins', data = {'amount_of_coins' : coins + 1})

await ctx.send('Added one coin!')
```

To delete an item from the database, use ``Database.delete_item()``

```py

db = await Database.delete_item(uid = ctx, database = 'coins')

coins = db.get('amount_of_coins', 0)

await ctx.send(f'Successfully got rid of all your coins. You had {coins} amount of coins!`)

```

### Database Types:
```py
Database.DatabaseType.USER # The Database will use User IDs as the primary key.

Database.DatabaseType.CHANNEL # The Database will use Channel IDs as the primary key.

Database.DatabaseType.GUILD # The Database will use Guild IDs as the primary key.

Database.DatabaseType.UNIVERSAL # The Database will not use a primary key, instead when called, data will be set universally. E.g. A Universal Count
```

## Performance Concerns?
While performance isn't significantly important for discord bots, this will write to a file and will loop through it to find values, meaning it will have a O(n) time complexitivity. This means that this extension isn't recommended for HUGE databases, use an actual database instead in those cases!
