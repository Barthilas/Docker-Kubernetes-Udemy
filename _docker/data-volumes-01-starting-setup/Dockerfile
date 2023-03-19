FROM node:14

# ARG DEFAULT_PORT=80
# ENV PORT $DEFAULT_PORT

EXPOSE $PORT
WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

# docker optimalization !LAYERS!
ARG DEFAULT_PORT=80
ENV PORT $DEFAULT_PORT

# VOLUME [ "/app/feedback" ] anonymous volume

# CMD [ "node", "server.js" ] doesnt watch for changes
CMD [ "npm", "start" ]

