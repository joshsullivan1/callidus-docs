Docker
*******

To setup the Callidus Mapping App using Docker, please follow these steps:

- Download and install `Docker <https://docs.docker.com/docker-for-windows/install/>`_
- Unpack the app and ``cd`` into the root directory
- Run ``docker-compose up``

Helpful Commands
-----------------

* ``docker ps -a`` will list all of the Docker containers
* ``docker exec -it <docker_container_name> bash`` will enter the container
* ``docker exec -it callidus_db_1 bash`` will enter the Postgres database container. When inside the container, run ``psql -U callidususer -W callidus`` and enter "youcantguess" at the password prompt. 
    
    * ``\l`` will list all the tables
    * ``\c callidus`` will change to the Callidus database
    * ``\dt`` will list the tables