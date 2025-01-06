FROM node:23-alpine AS build

WORKDIR /app

COPY . .

RUN yarn install
RUN yarn build

FROM node:23-alpine

WORKDIR /app

COPY --from=build /app/ .

EXPOSE 3000

ENTRYPOINT ["yarn", "start"]