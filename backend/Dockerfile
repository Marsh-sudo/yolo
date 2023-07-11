
#nodejs lightweight base image
FROM  node:18-alpine3.17  

#client workdir
WORKDIR /app/backend

#copying client dependancies
COPY package*.json ./

#install client dependancies
RUN npm ci --production

# Copy the rest of the project files
COPY . .

VOLUME /data/db

#run client project
CMD ["npm", "start"]
