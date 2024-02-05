# Beginner's Tutorial for Containerized Web Applications

[Link to Jonathan Harvey's tutorial](https://medium.com/@jharvey1012/a-beginners-guide-to-creating-a-containerized-web-application-development-environment-with-docker-e903169004ff)


## Updates to the tutorial
There are a couple of items in this tutorial that are not up to date:

- The Dockerfile in the `/server` folder, contains some code that installs PHP Compose. The code checks the hash sum for installer file. This hash needs to be up to date. The up-to-date value for the hash sum can be checked in this page: [https://getcomposer.org/download/](https://getcomposer.org/download/).
  ```
  RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02')
  # The hash sum above should be up to date
  ```

- When installing Laravel, we run some code inside the container. One of the commands fetches an example of a .env file from Github. The existing URL fetches the HTML from the Githum page, not the file itself. The correct URL to get the .env file is this: https://raw.githubusercontent.com/laravel/laravel/master/.env.example
  ```
  /# cd /var
  /# composer global update
  /# composer create-project --prefer-dist laravel/laravel server ; mv server/* www/ ; rm -rf server
  /# cd www ; chmod -R 775 storage
  /# wget https://raw.githubusercontent.com/laravel/laravel/master/.env.example <== Notice the URL here!
  /# cp .env.example .env
  /# php artisan key:generate
  /# exit

  ```

## Run the containers
In your terminal, navigate to `/myapp` and run `docker-compose up`. The servers will be available at `localhost:80` (client) and `localhost:8000` (server)

To run the containers individually, navigate to each folder and run the container:

```
  $ docker run -p 80:3000 --mount src=client,target=/var/www myclient:latest cd app; npm start
  $ docker run -p 8000:8000 --mount src=server,target=/var/www myserver:latest
```
