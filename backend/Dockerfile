FROM node

WORKDIR /app

COPY package.json /app

RUN npm install

copy . /app

EXPOSE 80

CMD ["npm","start"]