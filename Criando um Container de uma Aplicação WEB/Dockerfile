FROM node:16.19.1-alpine3.17 as build

USER node

RUN mkdir -p /home/node/app

WORKDIR /home/node/app

COPY --chown=node:node package*.json ./

RUN npm install

COPY --chown=node:node . .

RUN npm run build

FROM httpd:latest

COPY --from=build /home/node/app/website /usr/local/apache2/htdocs/