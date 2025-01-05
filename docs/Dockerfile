FROM node:23-alpine AS build

WORKDIR /app
COPY . .

RUN yarn install
RUN yarn build

FROM nginx:alpine

# Créer le groupe et l'utilisateur en premier
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Ajuster les permissions après la création de l'utilisateur et du groupe
RUN mkdir -p /var/cache/nginx /usr/share/nginx/html /var/run/nginx && \
    chown -R appuser:appgroup /var/cache/nginx /usr/share/nginx/html /var/run/nginx

# Copier les fichiers
COPY --from=build /app/build /usr/share/nginx/html

# Remplacer la configuration NGINX
COPY nginx.conf /etc/nginx/nginx.conf

USER appuser

EXPOSE 80
ENTRYPOINT ["nginx", "-g", "daemon off;"]