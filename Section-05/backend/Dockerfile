FROM node:lts-alpine3.22

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 8080

ENV MONGODB_USER=root

ENV MONGODB_PASSWORD=admin


#CMD ["node", "app.js"]
CMD ["npm", "start"]