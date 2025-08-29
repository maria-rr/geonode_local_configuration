# Basic GeoNode Deployment with Docker on Windows 
Basic configuration for a local geonode use

# Disclaimer
This is a basic configuration for testing geonode in a local mode, for more information please refer to the Geonode site documentation 
https://geonode.org/

# Prerequisites
- Have Docker installed and running on your machine.
- Ensure the following ports are free: 5432, 8000, and 8080

# Step 1: Verify Docker Installation
Open a terminal or console and run:
docker run hello-world
It should display the message: Hello from Docker!

# Step 2: In the working directory, clone the GeoNode project from GitHub
git clone https://github.com/GeoNode/geonode-project.git

# Step 3: In the working directory, clone the geonode_local_configuration files from GitHub
git clone https://github.com/maria-rr/geonode_local_configuration.git

# Step 4: Copy the .env and docker-compose.yml files 
Copy the .env and docker-compose.yml files from geonode_local_configuration to geonode-project root directory, this should be the same level as the Dockerfile is. Overwrite them if it is necessary.

# Step 5: In the root folder of geonode-project, locate the src folder
In the root folder of geonode-project, locate the src folder, you should have the following structure:
geonode-project/ \
├── src/ \
    ├── manage.py \
    ├── manage.sh \
    ├── setup.py \
    ├── uwsgi.ini \
    ├── project_name/ \
        ├── __init__.py \
        ├── apps.py \
        ├── celeryapp.py \
        ├── settings.py \
        ├── wsgi.py \

I'm not sure if there's a more elegant way to do this step :3, but you need to rename the project_name folder. In my case, I renamed it back to geonode-project. Then, within the listed files, you need to manually search and replace the string
{{ project_name }} with the name geonode-project.

# Step 6: Build the Docker container

With a cmd or bash terminal in the root folder of the geonode-project run:
docker compose --env-file .env -f docker-compose.yml build --no-cache

# Step 7: Start the Docker container
With a cmd or bash terminal in the root folder of the geonode-project run:
docker compose --env-file .env -f docker-compose.yml up

# Step 8: Run the migrations on the PostGIS database so that the required tables (such as django_site) are created.
If you keep the docker container, you should do this step once.

Open other cmd or bash terminal in the root folder of the geonode-project and run:
docker compose exec django bash

Then search for the manage.py file run:
find / -name manage.py

Copy the path or run:
cd /usr/src/app/src
python manage.py migrate

Once that finishes successfully, run:
python manage.py createsuperuser

Then create the superuser.

# Verification
Access http://localhost:8000
