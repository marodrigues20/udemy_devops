# Specify a base image
# node is my base image and 14-alpine is the tag
FROM node:14-alpine

# If this folder is not created. It will be created
WORKDIR /usr/app
# copy everything inside the current directory and paste inside the File System Container.
COPY ./package.json ./
# Install some dependencies
RUN npm install 
COPY ./ ./

# Default command
CMD [ "npm", "start"]