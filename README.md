# councilmatic-docker
Docker instance for councilmatic-scraper.  

## Quickstart

###  Make sure you have Docker installed for your OS
1. https://www.docker.com/

### Edit volumes docker-compose.yml for your local environment
1. Replace "<<local_db_data_dir>>" (Initially, this will be empty.)
2. Replace "<<local_councilmatic-scraper_git_repo_dir>>"
   1. If you don't have the latest copy of councilmatic-scraper, you can clone it from Github:
   ```
   git clone https://github.com/openoakland/councilmatic-scraper
   ```
   2. *Currently (12/13/17), we are working on the "events" branch. Switch use the "events" branch until further notice.
   ```
   cd councilmatic-scraper
   git checkout events
   ```
   
The local db data directory and the councilmatic scraper git repo directory cannot be subdirectories of each other. If you're still not sure what to set, you can take a look at docker-compose.yml.sample. This is how I have things set up on my computer on Mac OS X.

If you want, you can also change the POSTGRES_PASSWORD.  You will need this if you want to connect remotely to the database as "postgres".  The default postgres port, 5432, has been exposed.  You should be able to connect to the database on that port on 127.0.0.1 or the ip of your Docker host instance.

### Start docker instance with docker-compose
1. In directory with docker-compose.yml run:
```
docker-compose up
```

### Connect to your docker instance
1. Get the container id for your instance
```
docker ps
```
2. Log in as postgres
```
docker exec -it -u postgres <<container_id>> bash
```
3. Activate councilmatic-scraper virtualenv
```
source /home/postgres/councilmatic/bin/activate
```
4. cd into councilmatic-scraper repo directory
```
cd /home/postgres/work
```

### Initialize database (**Only run once**)
```
cd /home/postgres/scripts
sh setup_db.sh
```
This will create the opencivicdata database for you and init it for US.  You do not have to run "createdb opencivicdata" or "pupa dbinit us".  It probably won't do any harm but it will give you errors.

After you have initialized your database, files should appear in your local db data directory. 

### Run pupa update (for Oakland)
```
cd /home/postgres/work
pupa update oakland
```

The work directory should have the directory "oakland".  These Python files inside the directory have already been generated by "pupa init" and have been updated.

### Shut down your docker instance
```
docker-compose down
```

You should do this to shut down the docker instance and release the volume mounts cleanly.  Don't just do "docker stop..."