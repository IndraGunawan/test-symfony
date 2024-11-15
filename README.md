# test-symfony for DDEV testing only:

- Clone this repo, add upstream `git remote add upstream https://github.com/symfony/demo`
- Rebase the default branch `ddev-automated-test` onto the latest tag from `upstream`, resolve conflicts
- Use the [latest Symfony quickstart](https://ddev.readthedocs.io/en/latest/users/quickstart/#symfony) for this repo, do not commit `.ddev`
- `ddev exec "rm -rf assets/vendor var/sass vendor" && ddev composer install && git add assets/vendor var/sass vendor -f`
- Run `ddev console doctrine:schema:create && ddev console doctrine:fixtures:load --no-interaction`
- Export the db `mkdir -p .tarballs && ddev export-db --gzip=false --file=.tarballs/db.sql && tar -czf .tarballs/db.sql.tar.gz -C .tarballs db.sql`
- Run `git push`, create a new release adding `.tarballs/db.sql.tar.gz` as an asset
- Update the URLs in `ddev/ddev` for the new release (e.g. if the old release was `2.6.0` and the new release is `2.7.0`, then search for this string in the `ddev/ddev` and replace it with `2.7.0`)
- Rerun the tests for Laravel `GOTEST_SHORT=20 make testfullsitesetup`
