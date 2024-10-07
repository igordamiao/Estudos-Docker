FROM node:18 AS build

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

FROM node:18-alpine3.19 AS runtime

WORKDIR /app

COPY --from=build /app .

EXPOSE 3000

CMD ["npm", "start"]
