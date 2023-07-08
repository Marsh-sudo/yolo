#nodejs lightweight base image
FROM  node:14-alpine

#client workdir
WORKDIR /app/client

#copying client dependancies
COPY package*.json ./

#install client dependancies
RUN npm install

# Copy the rest of the project files
COPY . .

#EXPOSE port 3000
EXPOSE 3000

#run client project
CMD ["npm", "start"]
