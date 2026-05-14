FROM node:20-alpine

WORKDIR /app

COPY package.json package-lock.json .npmrc ./
RUN npm install --legacy-peer-deps

COPY . .

ENV MEDUSA_DISABLE_MEDUSA_ADMIN=true
RUN npm run build

ENV NODE_ENV=production
ENV MEDUSA_DISABLE_MEDUSA_ADMIN=true

EXPOSE 9000

CMD ["sh", "-c", "node_modules/.bin/medusa db:migrate && node .medusa/server/index.js"]
