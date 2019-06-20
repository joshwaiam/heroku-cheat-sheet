# heroku-cheat-sheet

A bunch of things I learned when deploying to Heroku!

## One repo, multiple Heroku apps

I like having my `api` and `client` folders in the same repo, but Heroku requires that they
are deployed to separate applications. In order to get this to work, you have to commit the subdirectories to the respective Heroku applications.

**Add and rename each Heroku app**

```typescript
// set git remote heroku to https://git.heroku.com/app-one-name.git
heroku git:remote -a app-one-name
// rename the remote to something else
git remote rename heroku heroku-app-one
```

**Add the publish script to the package.json in each subfolder**

```
"publish": "cd ../ && git subtree push --prefix api heroku-app-one master || true",
```

What is happening in the above script:

1. `cd ../`
   Moves up to the root directory.
2. `&& git subtree push --prefix api heroku-app-one master`
   Pushes the folder `api` to the `master` branch of the `heroku-app-one` repo.
3. `|| true`
   Suppresses an annoying NPM error

## Reset a Heroku repo

Sometimes your Heroku Git repo will get out-of-sync and you cannot push to it. Easiest way
to fix this is to reset it and push anew.

```typescript
// Install heroku-repo plugin
heroku plugins:install heroku-repo
// Reset the repo
heroku repo:reset -a yourappname
```

## Browse your deployed folder structure

If you want to browse your published Heroku app folder structure to debug build scripts `heroku run bash`.

## Monitor Heroku activity log

`heroku logs --tail -a yourappname`
