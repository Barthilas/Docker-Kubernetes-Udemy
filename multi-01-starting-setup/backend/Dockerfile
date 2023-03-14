FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

ENV MONGODB_USERNAME root
ENV MONGODB_PASSWORD secret

CMD ["npm", "start"]

# Preserve logs and allow live-reload with mount binding.
# Also specify anonymous container to not overwrite installed node_modules.
# docker run --name goals-backend -v logs:/app/logs -v :C:/Users/snetivy/Desktop/Docker Udemy/multi-01-starting-setup/backend/app -v /app/node_modules --rm -p 80:80 --network goals-net 
# -e MONGODB_USERNAME=admin -e MONGODB_PASSWORD=secret goals-node