# Python Setup Notes

Install Python27 + pip: https://github.com/BurntSushi/nfldb/wiki/Python-&-pip-Windows-installation

## Project Environment

Configure virtual environment
```
$ virtualenv projectname

$ source projectname/bin/activate : activates the created environment
```

```
$ pip freeze : checks what libraries are installed

$ pip install libraryname (Flask) : install lib in specific environment	

    deactivate : close the project environment

```

```
$ python -c "import flask" : imports a library on compile
```

## Run web app

```
$ export FLASK_APP=index.py 

$ export FLASK_ENV=development // enables debug mode

$ python -m flask run --host=0.0.0.0  | Makes our web-app public running using Flask

$ py -2 -m flask run --host=0.0.0.0 

$ curl -i -X POST http://webtech-11.napier.ac.uk:5000/account/ | Make a post from command line

$ curl -i -H "Content-Type: application/json" -X POST -d '{"title":"Read a book"}' http://webtech-11.napier.ac.uk:5000/api/tasks

$ curl -u root:python -i http://localhost:5000/todo/api/v1.0/tasks || Request with Basic Authentication
```

## Docker 

```
docker-compose -f docker-compose.yml up --build


sudo setfacl -m user:$USER:rw /var/run/docker.sock
SEE IMAGES: docker ps -a

docker run -p 8080:80 IMAGE_NAME

# if port exists already
docker container ls
docker rm -f <container-name>


rm -rf dirname-here
```

| NOTE: linux installation - https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04

## Git

```
$ git add .
$ git commit -m " Added introductory example "
$ git push
```