heroku-buildpack-contao
=======================

Heroku is great in deploying code with almost zero configuration. However, Heroku only can deploy
applications with a composer.lock file present.

This buildpack transforms the [`contao/contao`][1] monorepo into a runnable Contao managed installation
on the Heroku platform. Therefore, the buildpack creates a [boilerplate][2] Contao installation and
requires the source files as Composer dependency. Then it creates a `composer.lock` file.

More information on [Heroku buildpacks][3].

Usage:
------

Provide the following file inside the root of the monorepo. This [manifest][4] will load this buildpack
(whose README you are currently reading). Then, create a pipeline in the Heroku dashboard and enable
Git-triggered review apps for the repository.


```json
// app.json

{
  "addons": ["jawsdb-maria:kitefin"],
  "buildpacks": [
    {
      "url": "https://github.com/richardhj/heroku-buildpack-contao"
    },
    {
      "url": "heroku/php"
    }
  ]
}
```

This buildpack will provide a project boilerplate with composer.lock file, the `heroku/php` buildpack
then installs the Composer dependencies and boots the project.

On every build of the review app, —including new pushes to PRs—, the database schema will be updated as
it is defined in the [Procfile][5].


[1]: https://github.com/contao/contao
[2]: skeleton
[3]: https://devcenter.heroku.com/articles/buildpacks
[4]: https://devcenter.heroku.com/articles/app-json-schema
[5]: skeleton/Procfile
