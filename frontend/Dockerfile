FROM node

WORKDIR /app

COPY package.json /app

RUN npm install

copy . /app

EXPOSE 3000

CMD ["npm","start"]