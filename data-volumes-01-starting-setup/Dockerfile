FROM node:14
ENV PORT 80

EXPOSE $PORT
WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# VOLUME [ "/app/feedback" ] anonymous volume

# CMD [ "node", "server.js" ] doesnt watch for changes
CMD [ "npm", "start" ]

