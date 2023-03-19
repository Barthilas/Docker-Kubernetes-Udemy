FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD [ "npm", "start" ]

# the -it is important, else the container won't run.
# docker run --name goals-frontend --rm -p 3000:3000 -it --network goals-net
# -v C:/Users/snetivy/Desktop/Docker Udemy/multi-01-starting-setup/frontend/src:/app/src goals-react