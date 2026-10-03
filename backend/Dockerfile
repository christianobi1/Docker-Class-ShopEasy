FROM node:22

WORKDIR /app

# Update npm to pull patched npm dependencies
RUN npm install -g npm@latest

COPY package*.json ./
RUN npm install

COPY server.js db.js redisClient.js ./
COPY routes ./routes

RUN chown -R node:node /app

USER node

CMD ["node", "server.js"]
