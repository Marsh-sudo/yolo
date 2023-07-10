## Client Image
# To create a client image, we need to follow these steps:

- For the client image i decided to use node:14-alpine base image which is a lightweight image to keep the image size small. Alpine-based    images tend to have smaller sizes.

- Next we create a working directory for our container named /app/client

- Copy the client dependancies from package.json & package-lock.json into our container DIR.

- Use the `RUN` command to install the dependancies... We use npm `npm install` to install dependancies

- Next use the `COPY` directive to copy the whole of client project to our container DIR.

- Next exposing port 3000 using the `EXPOSE` directive

-  `CMD` directive used to run the container after image build