FROM node:23-alpine AS build

WORKDIR /app

COPY . .

RUN yarn install
RUN yarn build
RUN mv .next/server/pages /app

FROM node:23-alpine

WORKDIR /app

COPY --from=build /app/ .

EXPOSE 3000

ENTRYPOINT ["yarn", "start"]