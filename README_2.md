# SwIRL-Shared-Space

> This is the windows / generic adaptation for future SwIRL users, who may be less technically inclined

## About

This app was originally developed in Spring 2022 by a CSCE606 (Software Development) team at Texas A&M Univeristy. The original project is [on github](https://github.com/Einsgate/SwIRL-Shared-Space)

Maintenance has since been assumed by the employees and users of SwIRL.

> See [`Changes.md`](Changes.md) for an approximate list of changes made by SwIRL staff.

## Motivation

SwIRL serves many groups in various capacities. In order to help keep groups accountable for their messes as well as for addressing scheduling conflicts, this application has been developed. It consists of three main parts:

1. Organize users of the lab into groups based on their organization affiliation
1. The ability to reserve portions of SwIRL and note if the event will be noisy or will require silence
  1. The ability to make long term reservations on the storage areas within SwIRL
1. A simple system for tracking (Pass/Fail) completed machine shop trainings.


## Development

### Setup

#### Predicates

It is assumed that any user interested in the development will be comfortable with (or willing to learn) the following technologies:

1. Git - source control, via GitHub
1. Django - Python webapp framework (backend)
1. `pip` - python package manager
  1. `pipenv` - system for managing pip environments (selecting which items are 'installed', installing everything globally can occasionally lead to problems
1. `postgressql` - Database

#### Clone

Clone the github folder into a local environment on your computer

`git clone https://github.com/SouthwestIRL/SwIRL-Shared-Space`

> NOTE: all commands are presented as they would appear on a standard Windows configuration.
>
> If you are using a non-standard install or a Unix based OS, your commands may be different (but hopefully you are advanced enough to understand the significance)

#### Environment

__Install PostgreSql for your platform.__ 

Follow other online instructions. Take care to remember your password, it will be needed later. This database will be local to your machine and thus the password can be as secure or insecure as you deem necessary.

> Why PostgreSQL?
>
> Python / Django ship with a default database: SQLite. This is excellent locally but cannot be deployed to Heroku. Instead we must use PostgreSQL.

ON Windows, add `C:\Program Files\PostgreSQL\<version>\bin` to the `PATH` variable

> Remember to restart all command prompts to pickup changes to `PATH`

Then:

`psql -U postgres`

> if you need to change the login password:
`alter user postgres password <SHARED_SPACE_POSTGRES_PASSWORD>; `

`create database sharedspace;`

__Setup the environment:__

`pip install pipenv`
`pipenv shell`
`pipenv install`

#### Secrets

Secrets are stored as environment varaibles in the console session. That is, when the console exits, the secrets are destroyed. For testing purposes, the script `add_empty_secrets.bat` (`.sh` on Unix) will set all the secrets to `0`. This should allow the app to run, although functionality will be limited.

TODO - more info about secrets



#### Migrations

Migrations are how Django manages changes to the database structure. (still in the pipenv shell):

`manage.py migrate`

> Note this requires the secret `SHARED_SPACE_POSTGRES_PASSWORD` to be set to the correct value, otherwise the connection will be refused (for obvious security reasons)

#### Run the Server

`manage.py runserver`

At this point the server should be running on the local machine. 

TODO - why does logging in fail

### Testing


## Deployment

The application is hosted on Heroku (PaaS Company) ([link](https://dashboard.heroku.com/apps)). The login credentials for admin access can be distributed as necessary. 

Terms:

`dyno` - the name of a container for heroku 

### Heroku One Time setup:

* Install Heroku CLI (make sure that it is included in the PATH)
* `heroku login` (should open a browser window prompting login)

> TODO - how to link/join an existing app

### Heroku First Time setup:

* `heroku create -a swirl-shared-space`

Set the config vars (probably easiest on web):

* via cli `heroku config:set KEY=VALUE`

> Important!: any value set in teh configs will be visible to any user with permissions to edit the heroku app. They do not appear in the Git history.

> Note: you may need to restart the app and possibly even reset the database

### Deployment

* `git push heroku main`
* `heroku ps:scale web=1` - start an instance
* `heroku open` - view it

### Other Commands

See logs: `heroku logs --tail`
Reboot: `heroku restart`
See running dynos: `heroku list`
Enter a ssh bash prompt: `heroku run bash` (is this running locally?)

### The Nuclear Option

Reset the entire database and redeploy 

1. `heroku ps:scale web=0`
1. `heroku pg:reset`
1. Add a new commit:
   1. (optional: create empty commit) `git commit --allow-empty -m "Forcing Rebuild"`
   1. `git push heroku main`
1. `heroku ps:scale web=1`
1. `heroku open`
