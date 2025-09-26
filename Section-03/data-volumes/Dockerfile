FROM node:22.20-alpine

ARG DEFAULT_PORT=80

WORKDIR /app

COPY package.json /app

RUN npm install

COPY . .

ENV PORT=$DEFAULT_PORT

EXPOSE $PORT

#VOLUME [ "/app/feedback" ]

EXPOSE 80

CMD ["npm", "start"]