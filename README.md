# Lorekeeper

Lorekeeper is a framework for managing deviantART-based ARPGs/closed species masterlists coded using the Laravel framework. In simple terms - you will be able to make a copy of the site, do some minor setup/enter data about your species and game, and it'll provide you with the automation to keep track of your species, players and ARPG submissions.

- Demo site: [http://lorekeeper.me/](http://lorekeeper.me/)
- Wiki: [http://wiki.lorekeeper.me](http://wiki.lorekeeper.me/index.php?title=Main_Page)

# Features

- Users can create an account which will hold their characters and earnings from participating in the game.
- Mods can add characters to the masterlist, which can also record updates to a character's design. (Yes, multiple mods can work on the masterlist at the same time.)
- Characters get a little bio section on their profile that their owners can edit. Personalisation!
- Users' ownership histories (including whether they are an FTO) and characters' previous owners are tracked.
- Users can submit art to the submission queue, which mods can approve/reject. This dispenses rewards automagically.
- Users can spend their hard-earned rewards immediately, without requiring mods to look over their trackers (because it's all been pre-approved).
- Characters, items and currency can be transferred between users. Plus...secure trading between users for game items/currency/characters on-site is also a thing.
- Logs for all transfers are kept, so it's easy to check where everything went. 
- The masterlist is king, so ownership can't be ambiguous, and the current design of a character is always easily accessible.
- Speaking of which, you can search for characters based on traits, rarity, etc. Also, trait/item/etc. data get their own searchable lists - no need to create additional pages detailing restrictions on how a trait should be drawn/described.
- Unless you want to, in which case you can add custom pages in HTML without touching the codebase!
- A raffle roller for consecutive raffles! Mods can add/remove tickets and users who have already won something will be automatically removed from future raffles in the sequence.
- ...and more! Please refer to the [Wiki](http://wiki.lorekeeper.me/index.php?title=Category:Documentation) for more information and instructions for usage.

# Setup

Important: For those who are not familiar with web dev, please refer to the [Wiki](http://wiki.lorekeeper.me/index.php?title=Tutorial:_Setting_Up) for a much more detailed set of instructions!!

## Obtain a copy of the code

```
$ git clone https://github.com/lk-arpg/lorekeeper.git
$ git remote add ashtr3 https://github.com/ashtr3/lorekeeper.git
$ git fetch ashtr3
$ git merge ashtr3/extension/docker
```

## Configure .env in the directory

```
$ cp .env.example .env
```

Client ID and secret for at least one supported social media platform are required for this step. See [the Wiki](http://wiki.lorekeeper.me/index.php?title=Category:Social_Media_Authentication) for platform-specific instructions.

Add the following to .env, filling them in as required (also fill in the rest of .env where relevant):
```
CONTACT_ADDRESS=(contact email address)
DEVIANTART_ACCOUNT=(username of ARPG group account)
```

## Configure site.conf in the nginx directory

```
$ cp nginx/default.conf nginx/site.conf
```

You should update the server_name to match your site's domain name in the new site.conf file. If this container is being ran locally, you can keep this as `localhost`. 

```
server {
    listen 80;
    server_name (your domain name);
    ...
```

## Initialize the docker container

```
$ docker compose up -d --build
```

## Update filesystem permissions

```
$ docker compose exec app chown -R www-data:www-data /var/www/html/storage
$ docker compose exec app chown -R www-data:www-data /var/www/html/bootstrap/cache
$ docker compose exec app chmod -R 755 /var/www/html/storage
$ docker compose exec app chmod -R 755 /var/www/html/bootstrap/cache
```

These changes will enable the nginx user to create necessary changes to the `storage` and `bootstrap/cache` directories. This is required for log files.

## Setting up

Composer install:
```
$ docker compose exec app composer install
```

Generate app key and run database migrations:
```
$ docker compose exec app php artisan key:generate 
$ docker compose exec app php artisan migrate
```

Add basic site data:
```
$ docker compose exec app php artisan add-site-settings
$ docker compose exec app php artisan add-text-pages
$ docker compose exec app php artisan copy-default-images
```

Finally, set up the admin account for logging in:
```
$ docker compose exec app php artisan setup-admin-user
```

You will need to send yourself the verification email and then link your social media account as prompted.

## Contact

If you have any questions, please feel free to ask in the Discord server: https://discord.gg/U4JZfsu
