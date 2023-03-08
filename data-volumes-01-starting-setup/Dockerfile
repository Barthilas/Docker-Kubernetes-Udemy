FROM node:14

EXPOSE 80
WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# VOLUME [ "/app/feedback" ] anonymous

CMD [ "node", "server.js" ]
