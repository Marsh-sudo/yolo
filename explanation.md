## Client Image
# To create a client image, we need to follow these steps:

- For the client image i decided to use node:14-alpine base image which is a lightweight image to keep the image size small. Alpine-based    images tend to have smaller sizes.

- Next we create a working directory for our container named /app/client

- Copy the client dependancies from package.json & package-lock.json into our container DIR.

- Use the `RUN` command to install the dependancies... We use npm `npm ci` to install dependancies

- Next use the `COPY` directive to copy the whole of client project to our container DIR.

- Next exposing port 3000 using the `EXPOSE` directive

-  `CMD` directive used to run the container after image build

# Backend image
- For the backend image i decided to use node:14-alpine base image which is a lightweight image to keep the image size small. Alpine-based    images tend to have smaller sizes.

- Next we create a working directory for our container named /app/backend

- Copy the client dependancies from package.json & package-lock.json into our container DIR.

- Use the `RUN` command to install the dependancies... We use npm `npm ci` to install dependancies

- Next use the `COPY` directive to copy the whole of backend project to our container DIR.

- Next exposing port 3000 using the `EXPOSE` directive

- Next we create volumes using `VOLUMES` directive as this is where we want to store our volumes for our backend container

-  `CMD` directive used to run the container after image build

I used `npm ci` as we want to make sure you’re doing a clean install of your dependencies. It can be significantly faster than a regular npm install by skipping certain user-oriented features. It is also more strict than a regular install, which can help catch errors or inconsistencies caused by the incrementally-installed local environments of most npm users.

- Find images at docker hub: https://hub.docker.com/u/marshkelvin0 named client && backend images

## Docker-Compose
- For the docker compose, I created two services using the client and backend containers I created.

- For the volumes, I logically used docker volumes in the backend service as it is responsible for the data managing(Mongodb). The /data/ db  is the target or destination of the data being saved by your volume 

- For the networks, both services are using `yoloProduct` network. The network will be created when the docker compose runs at first. The services use different port mapping but this will not affect the communication between the two services as they are using the same network to communicate.