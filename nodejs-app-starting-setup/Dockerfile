# Don't run a image, run a container based on image!
# Image is the template for the container.

# use image
FROM node 

WORKDIR /app

# copy all folder/subfolders/project inside the image (internal filesystem)
# COPY . /app
# With workdir set this can now be set as.
# COPY . ./
COPY . /app

# gacha executed in working dictionary which is root -> change workdir.
RUN npm install

# Expose port to listen on the container.
EXPOSE 80

# ! CMD VS RUN READ FIRST ROWS.
# TLDR: Use run to set up image, and cmd to execute container.
# RUN node server.js

# Without CMD the CMD of the base image wil be executed. With no base image to begin with you get an error.
# Should be the last command of dockerfile.
CMD ["node", "server.js"]


