# Ruby on Rails on FridayBuilds

## Prerequisites

- Ruby 3.2.2
- Postgres

## Setup

Copy this repository to your local machine, install rails and create a database: 
```
git clone https://github.com/Tashows/rails-on-friday.git
cd rails-on-friday
```

The following commands will generate a new `master.key` file in your `config` directory and a `credentials.yml.enc` file that will be used to store your secrets. You can use any text editor you like, just replace `nano` with your preferred editor.

```
rm config/credentials.yml.enc
EDITOR=nano rails credentials:edit
```

Next run the following commands to install the required gems and create the database:

```
bundle install
rails db:create db:migrate
```

If you want to test locally, you can run the following command:
```
rails s
```
and then visit `http://localhost:3000` in your browser. You should see a welcome page with the Rails logo and version details at the bottom.

## Deploy on FridayBuilds.com

1. Create an account on [FridayBuilds](https://fridaybuilds.com)
2. Create a new app from the FridayBuilds dashboard. Choose a `slug` that will be used to identify your app. You will need it later.
3. Add a postgres database service to your app from your app's management page.
4. Add a new environment variable for your app from your app's settings page. The key should be `RAILS_MASTER_KEY` and the value should be the contents of your `config/master.key` file. You can find the contents of this file by running `cat config/master.key` in your terminal.
5. Your app is now ready and configured. On your local machine, run the following commands:

```
gem install sudokku_cli
sudokku login
```

At this point you have access to your app's private git repository on FridayBuilds. You can push your code to this repository and it will be automatically deployed.
In the following commands, replace `YOUR_APP_SLUG` with the `slug` you chose earlier.

```
git remote add fridaybuilds https://git.fridaybuilds.com/YOUR_APP_SLUG.git
git push fridaybuilds main
```

Once your deployment is complete you will want to locate the "Remote Commands" section in your app's settings page. In the input box, type `rails db:migrate` and click "Run" or press enter.
This will run the migrations on your database, so that rails can properly connect to it.

If you later want to deploy a new version of your app, you can simply commit your changes and push them to the FridayBuilds repository.
After you change something in your code, you can run the following commands:

```
git add .
git commit -m "My changes"
git push fridaybuilds main
```

A new deployment will be triggered and your app will be updated with the new code.

Happy FridayBuilding!
