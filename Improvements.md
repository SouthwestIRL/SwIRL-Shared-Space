# Desired Improvements

* Ability to test locally with SQLite and only use PostgreSQL when deployed to heroku

* TODO - why do we need to disable `collect_static`?
  * `heroku config:set DISABLE_COLLECTSTATIC=1`

* TODO - explore `heroku local`

* The admin password is defined in a migrations file and based off the environment variable (thus it does not change with the environment variable).

* Cannot create an account: some error about name

* Google URI redirect error
