# Lucky Framework Howto

## Configure Vim

[Ref](https://github.com/vim-crystal/vim-crystal)

### `.vimrc`

```
$ nano ~/.vimrc
:set tabstop=2 shiftwidth=2 expandtab
```

### Crystal Syntax Highlight

```
# Vim8
$ mkdir -p ~/.vim/pack/crystal/start
$ cd ~/.vim/pack/crystal/start
$ git clone https://github.com/vim-crystal/vim-crystal.git
```

## Configure Git

```
$ git config --global user.email "kun.liu@outlook.com"
$ git config --global user.name "kun.liu"
```

## Install Lucky on Ubuntu 22.04

[Ref](https://luckyframework.org/guides/getting-started/installing)

### Install Dependencies

```
$ sudo apt update
$ sudo apt install libc6-dev
$ sudo apt install libevent-dev
$ sudo apt install libpcre2-dev libpcre3-dev
$ sudo apt install libpng-dev
$ sudo apt install libssl-dev
$ sudo apt install libyaml-dev
$ sudo apt install zlib1g-dev
$ sudo apt install build-essential pkgconf
$ which cc
$ which gcc
$ sudo reboot
```

### Install CLI Manually

`*** PLEASE INSTALL CRYSTAL FIRST ***`

```
# Clone the CLI repo
$ git clone https://github.com/luckyframework/lucky_cli

# Change into the newly cloned directory
$ cd lucky_cli

# Check out the latest released version
$ git checkout v1.1.0

# Install shards
$ shards install --without-development

# Build the CLI
$ shards build --production

# Move the generated binary to your path
$ sudo cp bin/lucky /usr/local/bin

# Check installation
$ lucky -v
```

### Install CLI via Homebrew

#### Install Homebrew

```
$ vi ~/.profile
export PATH="/home/linuxbrew/.linuxbrew/bin:$PATH"
$ source ~/.profile

$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
$ brew update
```

#### Install CLI

```
$ brew update
$ brew install luckyframework/homebrew-lucky/lucky
$ which lucky
$ lucky -v

$ sudo update-alternatives --install /usr/bin/gcc gcc /home/linuxbrew/.linuxbrew/bin/gcc-13 13 --slave /usr/bin/g++ g++ /home/linuxbrew/.linuxbrew/bin/g++-13 --slave /usr/bin/gcov gcov /home/linuxbrew/.linuxbrew/bin/gcov-13
$ sudo update-alternatives --config gcc
$ which gcc
$ gcc --version

$ vi ~/.profile
export PATH="/home/linuxbrew/.linuxbrew/opt/llvm@15/bin:$PATH"
export PATH="/home/linuxbrew/.linuxbrew/bin:$PATH"
$ source ~/.profile
$ which clang
$ clang --version
```

## Install Lucky on MacOS

[Ref](https://luckyframework.org/guides/getting-started/installing)

```
# Install Homebrew
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install OpenSSL
$ brew install openssl

# Configure SSL
# Apple M1
$ echo 'export PKG_CONFIG_PATH=/opt/homebrew/opt/openssl/lib/pkgconfig' >>~/.zshrc
$ echo 'export PKG_CONFIG_PATH=/opt/homebrew/opt/openssl/lib/pkgconfig' >>~/.bash_profile
# Apple Intel
$ echo 'export PKG_CONFIG_PATH=/usr/local/opt/openssl/lib/pkgconfig' >>~/.zshrc
$ echo 'export PKG_CONFIG_PATH=/usr/local/opt/openssl/lib/pkgconfig' >>~/.bash_profile

# Install Lucky CLI
$ brew install luckyframework/homebrew-lucky/lucky
$ lucky -v
```

## Create New Project

### Generate an API app

```
$ lucky init.custom <projname> --api --dir /home/wayahead/windmill/projs
Done generating your Lucky project

  ▸ cd into /home/wayahead/windmill/projs/usrmgr
  ▸ check database settings in config/database.cr
  ▸ run script/setup
  ▸ run lucky dev to start the server

$ cd /home/wayahead/windmill/projs/<projname>
$ vi config/database.cr
$ script/setup
$ lucky dev
```

```
$lucky -v
1.1.0
$ lucky tasks
Usage: lucky name.of.task [options]

Available tasks:

  ▸ db.console             Access PostgreSQL console
  ▸ db.create              Create the database
  ▸ db.drop                Drop the database
  ▸ db.migrate             Run any pending migrations
  ▸ db.migrate.one         Run just the next pending migration
  ▸ db.migrations.status   Print the current status of migrations
  ▸ db.redo                Rollback and run just the last migration
  ▸ db.reset               Drop, recreate, and run migrations.
  ▸ db.rollback            Rollback the last migration
  ▸ db.rollback_all        Rollback all migrations
  ▸ db.rollback_to         Rollback to a specific migration
  ▸ db.schema.dump         Export database schema to a sql file
  ▸ db.schema.restore      Restore database from a sql dump file
  ▸ db.seed.required_data  Add database records required for the app to work
  ▸ db.seed.sample_data    Add sample database records helpful for development
  ▸ db.setup               Runs a few tasks for setting up your database
  ▸ db.verify_connection   Verify connection to postgres
  ▸ exec                   Execute code. Use this in place of a console/REPL
  ▸ gen.action.api         Generate a new api action
  ▸ gen.action.browser     Generate a new browser action
  ▸ gen.component          Generate a new HTML component
  ▸ gen.email              Generate a new Email
  ▸ gen.migration          Generate a new migration
  ▸ gen.model              Generate a model, query, and operations (save and delete)
  ▸ gen.page               Generate a new HTML page
  ▸ gen.resource.browser   Generate a resource (model, operation, query, actions, and pages)
  ▸ gen.secret_key         Generate a new secret key
  ▸ gen.task               Generate a lucky command line task
  ▸ routes                 Show all the routes for the app
  ▸ watch                  Start and recompile project when files change
```

### Generate a Full Web app

```
$ lucky init.custom <appname> --dir /home/wayahead/windmill/projs
Done generating your Lucky project

  ▸ cd into /home/wayahead/windmill/projs/usrmgr
  ▸ check database settings in config/database.cr
  ▸ run script/setup
  ▸ run lucky dev to start the server

$ cd /home/wayahead/windmill/projs/<projname>
$ vi config/database.cr
$ script/setup
$ lucky dev
```

## Configure Environment

### Set Environment Varaiable

```
$ export LUCKY_ENV=<environment_name>
$ export LUCKY_ENV=development
$ export LUCKY_ENV=test
$ export LUCKY_ENV=production
```

### Check Environment in Code

```
LuckyEnv.environment: return current running environment
LuckyEnv.development?: check whether development environment
LuckyEnv.test?: check whether test environment
LuckyEnv.production?: check whether producton environment
```

### Add Custom Environment

```
$ vi <appname>/config/env.cr
LuckyEnv.add_env :staging
LuckyEnv.staging? # => false
```

### Configure in Code

```
# you may need a model when you want configuration
# src/models/my_stripe_processor.cr
class MyStripeProcessor
  Habitat.create do
    setting api_token : String
  end
end

# you can configure in config directory
# config/my_stripe_processor.cr
MyStripeProcessor.configure do |settings|
  settings.api_token = "super secret"
end

# you can set .env to have environment variable
# .env
# this will be loaded in development when you call `lucky dev`
STRIPE_API_TOKEN=123abc

# then you can get configuration from environment variable
# config/my_stripe_processor.cr
MyStripeProcessor.configure do |settings|
  settings.api_token = ENV.fetch("STRIPE_API_TOKEN")
end

# remember to set environment variable in production env.
```

## Configure Database

```
# Lucky needs postgresql user who is able to create database
# so you can use super user `postgres` or create new superuser
$ vi config/database.cr
```
