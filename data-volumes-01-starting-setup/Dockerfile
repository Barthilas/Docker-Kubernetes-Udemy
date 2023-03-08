FROM node:14

EXPOSE 80
WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# VOLUME [ "/app/feedback" ] anonymous volume

CMD [ "node", "server.js" ]
