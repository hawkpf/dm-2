Digital Mappa v2.0 
============================================

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy)

Digital Mappa v2.0 (DM2 for short) is a freely available online environment for creating projects out of digital images and texts. The premise of DM2 is simple and powerful: if you have a collection of digital images and/or texts, you should be able to produce an online resource that links together specific moments on these images and texts together, annotate these moments as much as you want, collaborate with others on this work, have the content you produce be searchable, and publish this work to others or the public as you wish. And you should be able to do this with little technical expertise.

DM2 was developed under the direction of Martin Foys and his team at the University of Wisconsin-Madison and Dot Porter at the Schoenberg Institute for Manuscript Studies. Funding was provided through a grant from the National Endowment for the Humanities and through funding from UW Madison. Performant Software Solutions LLC (www.performantsoftware.com) performed the software development, with Andy Stuhl and Nick Laiacona being the primary contributors to the 2.0 release. 

DM2 design was inspired by the DM project (https://github.com/performant-software/DM) developed originally at Drew University by Martin Foys and others.


Technical Overview
---------------

DM2 is a single page React application backed by a Ruby on Rails server running a Postgres database. It uses ActiveStorage for image uploads and ImageMagick for image processing. It utilizes the SendGrid service for outbound SMTP and Amazon S3 for image storage. It has been developed within the Heroku (heroku.com) environment but has no Heroku specific dependencies. Issues are tracked and relases are issued on the GitHub repo at https://github.com/performant-software/dm-2 . 


Heroku Installation
-------------

<<<<<<< HEAD
To install DM2 on Heroku, create a new app and point it at this respository. You will need to provision SendGrid and Heroku PostGres. The following config variables should be set for the application:

* AWS_ACCESS_KEY_ID
* AWS_BUCKET
* AWS_REGION
* AWS_SECRET_ACCESS_KEY
* HOSTNAME
* LANG
* RACK_ENV
* RAILS_LOG_TO_STDOUT
* RAILS_SERVE_STATIC_FILES
* SENDGRID_PASSWORD
* SENDGRID_USERNAME

You will also need to provision an Amazon S3 bucket to store the uploaded image files and configure access using Amazon IAM. Once a S3 bucket has been created you will need to set Cross-origin resource sharing (CORS) in the permissions tab of the S3 bucket.  See aws.amazon.com for more information.
=======
### Create app

To install DM2 on Heroku, create a new app and point it at this respository, using the following command with the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli):

```sh
heroku create --stack heroku-18
```

If you did not provision your app using the Heroku CLI, you may need to manually switch the stack to `heroku-18`, as this app currently relies on a version of Ruby that may not be supported by the current default stack (`heroku-20` at the time of writing). This can be done with the following command:

```sh
heroku stack:set heroku-18
```

and will be activated at next build. For more information, see [Heroku-18 Stack](https://devcenter.heroku.com/articles/heroku-18-stack) and [Heroku Ruby Support](https://devcenter.heroku.com/articles/ruby-support#ruby-versions).

You will also need to activate both the Ruby and Node.JS buildpacks. This can be done from the Heroku CLI:

```sh
heroku buildpacks:set heroku/ruby
heroku buildpacks:add --index 1 heroku/nodejs
```

### Provision resources

You will need to provision SendGrid and Heroku Postgres using the Heroku Resources section.

You will also need to provision an Amazon S3 bucket to store the uploaded image files and configure access using Amazon IAM. Once a S3 bucket has been created you will need to set Cross-origin resource sharing (CORS) in the permissions tab of the S3 bucket. See https://aws.amazon.com/ for more information.

### Configuration variables

The following config variables should be set for the application:

```
AWS_ACCESS_KEY_ID
AWS_BUCKET
AWS_REGION
AWS_SECRET_ACCESS_KEY
EMAIL_FROM
HOSTNAME
LANG
PROTOCOL
RACK_ENV
RAILS_LOG_TO_STDOUT
RAILS_SERVE_STATIC_FILES
SECRET_KEY_BASE
SENDGRID_PASSWORD
SENDGRID_USERNAME
```
>>>>>>> 9b2b8462ad2c0c49eca6b4e02e6309bbb882f000

Here are some default settings for provisioning a production server:

```env
LANG=en_US.UTF-8
RACK_ENV=production
RAILS_ENV=production
RAILS_LOG_TO_STDOUT=enabled
RAILS_SERVE_STATIC_FILES=enabled
PROTOCOL=https
```

The `SECRET_KEY_BASE` environment variable is used to encrypt the passwords on your DM2 instance, so it is important to keep it secure and unguessable. Here's a good site for generating a secret key: https://www.grc.com/passwords.htm

Set the `HOSTNAME` and `PROTOCOL` environment variables to the hostname and protocol of your Heroku application. For example, if your application is hosted at `https://my-project.herokuapp.com`, you would set the `HOSTNAME` variable to `my-project.herokuapp.com`, and the `PROTOCOL` variable to `https`.

The `EMAIL_FROM` environment variable is used for sending emails via SendGrid. This should be set to the email address you would like to appear in the "From" field in registration confirmation emails. Additionally, SendGrid has changed their authentication scheme from username/password to API keys. Thus, to set the SendGrid environment variables, you must go to your provisioned SendGrid account from the Heroku dashboard, and find the "Settings" > "API Keys" section of the SendGrid service. Click the "Create API Key" button, copy the created key to the `SENDGRID_PASSWORD` environment variable, and set the `SENDGRID_USERNAME` environment variable to `apikey`.

```env
SENDGRID_USERNAME=apikey
SENDGRID_PASSWORD=SG.abcdefghijklmnopqrstuvwxyz
```

By default, the production environment will use AWS as the Active Storage service. This will require the following environment variables to be set:

```
AWS_ACCESS_KEY_ID
AWS_BUCKET
AWS_REGION
AWS_SECRET_ACCESS_KEY
```

It is possible use local storage, however this is only recommended for testing purposes, as Heroku does not have a persistent file system. This can be done by setting the `ACTIVE_STORAGE_SERVICE` variable to "local".

### Set up database

Once these things are done, migrate the database using the following command:

```
heroku run rails db:migrate && heroku run rails db:seed
```

DM2 should now be up and running on your Heroku instance! 

The first user account created is automatically given admin powers. Thereafter, that user can grant other users access and privledges using the Admin menu in the top right corner of the interface. 


Heroku Local Development Environment 
-------------

DM2 is a pretty standard Ruby on Rails 5.x application. It uses a PostgreSQL and has been developed using PostgreSQL v11.1. It was developed using Ruby 2.5.7 and Bundler 2.2.23. Setting up PostgresSQL, Ruby, and Bundler are beyond the scope of this README, but plenty of information is available online about these tools.

Once the dependencies mentioned above are installed, please follow these steps:

1) Clone this repo to your local drive:

```sh
git clone https://github.com/performant-software/dm-2.git
```

2) Run bundler in the base directory to get all the Ruby dependencies:

```sh
bundle 
```

3) Run yarn in the client directory to get all the JS dependencies:

```sh
cd client
yarn
```

4) Create a database for the application. The default database is called "dm2_staging" with no username or password. You can configure this in the config/database.yml file. Once the database is created, run:

```sh
rails db:migrate
```

5) Run the server with the following command:

```sh
heroku local -f Procfile.dev
```

Note that this runs two servers, one on port 3000 for Ruby on Rails and one on 3001 for the Create React App yarn server. This hot reloads any changes made to the Javascript files as you develop.

6) Visit http://localhost:3000 to view the application. 

Please note that the development environment stores files on local disk in the /storage directory by default. You can configure different storage solutions in config/storage.yml. See the Rails ActiveStorage documentation for more details.

Installation without Heroku Toolset
-------------

Installation without the Heroku tool set is possible but requires setup specific to your enviroment. Follow the steps given above, except when it comes time to run the application, run the client and the server with these commands:

To run the client:
   
```sh
cd client && PORT=3000 yarn start
```

Then run the server:
   
```sh
PORT=3001 && bundle exec puma -C config/puma.rb
```

Active Storage
-------------

Active storage is used to handle the uploading/downloading of files. Files can either be stored locally on the disk or on a file storage service, such as Amazon S3. For ease of toggling in different environments, the file storage service can be specified as an environment variable in the `application.yml` file.

    ACTIVE_STORAGE_SERVICE: 'local'
    ACTIVE_STORAGE_SERVICE: 'amazon'
    
To convert an existing application to use a different file storage service, a rake task exists to downloading the files from the current storage and upload them to the new storage:

    bundle exec rake active_storage:aws_to_local
    bundle exec rake active_storage:local_to_aws
