FROM node:current-alpine
WORKDIR /usr/src/app

COPY package*.json .
COPY rollup.config.js .
COPY tsconfig.json .

RUN npm install