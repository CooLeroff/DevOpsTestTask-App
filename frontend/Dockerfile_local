FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

ENV REACT_APP_BACKEND_URL=http://localhost

CMD ["npm", "start"]