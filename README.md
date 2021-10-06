It's a test project. Deployed to Amazon EC2, with Amazon RDS PostgreSQL database and Let's Encrypt certificate

https://blog.senabo.space/

Stack: <br/>
Python / Django Rest Framework for API <br/>
JS/ React-Redux for Frontend <br/>
Docker for container <br/>
Gunicorn and nginx for production server<br/>



# Instruction 

---

## Development

Uses `docker-compose.yml` with `nginx-dev.conf`.

* create `.env`  from `.env.sample`:
    
    ```sh
    cp .env.sample .env
    ```

* update the environmental variables in the `.env` file.

* build the images and run the containers:

  ```sh
  docker-compose up --build  
  ```
  Test it out at http://localhost.

## Production
Uses `docker-compose-prod.yml` with `nginx-prod.conf`. 

It set up to deploy on EC2 (AWS) with Let's Encrypt certificate. 
You should have an own domain name.

* create `.env`  from `.env.sample`:

    ```sh
    cp .env.sample .env
    ```

* update the environmental variables in the `.env` file.
* update `nginx-prod.conf`.
* update `init-letsencrypt.sh`:
  * update `domains` variable
  * set `staging` to `1` to test the configuration first with Let’s encrypt staging environment! 
  * set `compose_file` (in this project: `docker-compose-prod.yml`)
* build containers:
  ```sh
  sudo docker-compose -f docker-compose-prod.yml build
  ``` 
* Let's issue certificate from Let’s Encrypt staging environment. 
  ```sh
  sudo ./init-letsencrypt
  ``` 
  If successful you should be able to navigate to your website https://example.com (change to your domain). You`ll see the warning message the will tell you that connection is insecure. To see the website, you need to go into advanced options and accept the risk.
* If everything is ok you should stop containers:
  ```sh
  sudo docker-compose -f docker-compose-prod.yml down
  ```
* You need to edit `init-letsencrypt.sh` file and set `staging=0`. Then run the script:
  ```sh
  sudo ./init-letsencrypt.sh
  ```
  
* And finally stop containers and run them in the background:
  ```sh
  sudo docker-compose -f docker-compose-prod.yml down
  sudo docker-compose -f docker-compose-prod.yml up --detach
  ```
  
---

I want to thank the authors of these articles:

https://medium.com/swlh/how-to-deploy-django-rest-framework-and-react-redux-application-with-docker-fa902a611abf

https://saasitive.com/tutorial/docker-compose-django-react-nginx-let-s-encrypt/