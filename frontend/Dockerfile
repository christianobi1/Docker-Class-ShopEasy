# Build
FROM node:20 AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

RUN npm run build

# Runtime
FROM nginx:alpine

# Create a non-root user
RUN addgroup -S nginxapp && adduser -S nginxapp -G nginxapp \
    && chown -R nginxapp:nginxapp /usr/share/nginx/html \
    && chown -R nginxapp:nginxapp /var/cache/nginx \
    && chown -R nginxapp:nginxapp /var/log/nginx \
    && chown -R nginxapp:nginxapp /etc/nginx/conf.d \
    && touch /var/run/nginx.pid \
    && chown nginxapp:nginxapp /var/run/nginx.pid

COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 8080

USER nginxapp

CMD ["nginx", "-g", "daemon off;"]
