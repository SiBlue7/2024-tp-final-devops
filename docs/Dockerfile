FROM node:23-alpine AS build

WORKDIR /app
COPY . .

RUN yarn install
RUN yarn build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

USER appuser

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]