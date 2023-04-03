# Airflow-SSO

## Downloading the airflow image
In this example I will use a standard airflow docker image, which will be available at the following link in the airflow documentation->
`https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html`


## Setting up Google OAuth credentials
After following the instructions for using the docker image you will need to set up an OAuth app on google.


Access the google developer panel at the link ->
`https://console.developers.google.com/?hl=pt-br`

- Create a project
### OAuth Permission Screen
- Go to "OAuth Permission Screen"
 - Select external option
 - Provide your app name and support email, if you have a logo you can provide that too
 - In "app domain" provide the following urls:
    (even if some of these domains are not in use, it is good to mention them to avoid complaints from google!)
    <img src="https://user-images.githubusercontent.com/63692868/229544149-65c5bdea-20d3-4aeb-b775-dcf52bbd2301.png" height="100px" width="720px" />
 
 - In "authorized domains" leave null.
 - Press save and continue
 - In "add or remove scopes" select the first two options
    <img src="https://user-images.githubusercontent.com/63692868/229545601-f0325189-3e47-451a-bc21-56cac9c0379f.png" height="100px" width="720px" />
    - Save and continue
 
 - Save your test users and press save and continue

### Credentials
 - Go to "CREATE CREDENTIALS"
   - Select "OAuth Client ID"
   - Select a option "Web App"
 - In "Authorized JavaScript Sources" provide the following urls
    - <img src="https://user-images.githubusercontent.com/63692868/229549221-414a37c8-0eda-45a1-9b8c-0434325138c5.png" height="100px" width="720px" />

    - `http://localhost:8080`
    - `http://localhost`
 - In "Authorized Redirect URIs" provide the following urls
    -  <img src="https://user-images.githubusercontent.com/63692868/229549935-94ea0957-f364-4cb2-84b5-26fde0702706.png" height="100px" width="720px" />
    - `http://localhost:8080`
    - `http://localhost`
    - `http://localhost:8080/oauth-authorized/google`
- Save and copy your generated credentials, "ID key" and "secret key".


## Editing the airflow docker environment


After making the google settings, we will make the necessary settings in the docker image so that it can make the access requests to google.

Check if your airflow webserver container is running, then get the id of your airflow webserver container (you can get the ID by running the `docker ps` command).

Then we will run the following command to enter the root directory where the files for modification will be available

(Note: I'm using vscode with the Docker extension, I recommend it to make the process easier!)
### AIrflow.cfg
Open the airflow.cfg file as in the image and modify the following two lines as in the example:
 
 - look for the [webserver] section, if the lines do not exist create them as follows:
    - `x_frame_enabled = False`
    - `warn_deployment_exposure = False`
    - if they exist just change the value to false
    <img src="https://user-images.githubusercontent.com/63692868/229558161-3daa3188-ba5e-464b-a3e7-16a5449bf971.png" height="100px" width="720px" />


### webserver_config.py

Now let's go to the webserver_config.py file, do it as shown in the image

<img src="https://user-images.githubusercontent.com/63692868/229562278-5db88561-2f6d-474a-a385-7f1562d2b730.png" height="100px" width="720px" />

Note: you can mention the keys as a string directly in the code (not recommended) or create an .env file and export the variables and mention them in the python files.


After making all the changes, restart your airflow webserver container with `docker ps <container-id>`, go to your localhost:8080 and you will be able to see that authentication with google is already available.

see the pictures:

<img src="https://user-images.githubusercontent.com/63692868/229563486-13653bb8-d8e6-43c5-bfd0-75bca1972dba.png" height="100px" width="720px" />
<img src="https://user-images.githubusercontent.com/63692868/229563570-8f6b20ee-0ecc-4c16-96e4-4d8bbf70ddf6.png" height="100px" width="720px" />
<img src="https://user-images.githubusercontent.com/63692868/229563686-344d9c94-4589-49c8-a9ce-867446044f21.png" height="100px" width="720px" />




links used from airflow documentation:
https://airflow.apache.org/docs/apache-airflow/stable/administration-and-deployment/security/webserver.html
https://flask-appbuilder.readthedocs.io/en/latest/search.html?q=helm+&check_keywords=yes&area=default