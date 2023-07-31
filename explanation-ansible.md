## Yolo App Ansible
# To deploy our Yolo e-commerce app on Ubuntu servers powered by vagrant we follow these steps:

- First we create an ansible playbook where we will add the plays to be performed on our Ubuntu servers

- We set `become` is yes this is to allow our playbook to perform tasks as the root user on our servers.

- We defined variables to be used using the `vars` module where we defined the location of where app will be located on the servers eg `/Devops/yolo_app` and also set a variable for our github repo which ansible will clone. `github_repo: "https://github.com/Marsh-sudo/yolo"`

# Roles
- For the roles i created a directory called roles where we will define where our ansible roles will be defined

- The first role is named nodejs; This role will be incharge in installing nodejs and npm dependancies on our servers. But first we use `apt` package manager to install nodejs.

- We use `apt` because we are using Ubuntu servers and `apt` is responsible for downloading any packages on Ubuntu


1) Install and configure Docker - Client Dockerfile;
- This role is incharge of building the client docker image and also build the container.

- We install docker using `apt` and enable the docker service.

- Next we create a play that will build the docker images. We use the `docker_image` ansible module. Give the image a name `client` and the set the path of where ansible will find the Dockerfile

- Building of the client container we use an ansible module `docker_container` to run the container. We set which image ansible will use to build the container; `client` image. Give the container a name and use the state `present` to start the container.

2) Backend Dockerfile Role - This role is incharge of building the backend docker image.  

- We install docker using `apt` and enable the docker service.

- Next we create a play that will build the docker images. We use the `docker_image` ansible module to build the image. Give the image a name `backend` and the set the path of where ansible will find the Dockerfile

- Building of the backend container we use an ansible module `docker_container` to run the container. We set which image ansible will use to build the container; `backend` image. Give the container a name and use the state `present` to start the container.


- On our ansible playbook, We install packages `git` & `curl` using the apt package manager

- We clone the Github repo using the installed `git` module. Here we use the variables we defined earlier in the `vars` section stating the repo link and also the destination of where to clone the repo in our case. 

- We use ansible module `file` to make sure that our yolo project exists in path set in our case `path={{ yolo_location }}`

- Next we start our docker-compose using the `command` module as here we use the docker compose command; `docker-compose -f {{ yolo_location }}/docker-compose.yml up -d` where the `{{ yolo_location }}/docker-compose.yml` is the location of our YAML docker-compose file for running docker compose. 