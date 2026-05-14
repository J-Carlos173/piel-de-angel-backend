FROM node:20-alpine

WORKDIR /app

COPY apps/backend/package.json apps/backend/package-lock.json ./
RUN npm install --legacy-peer-deps --include=dev

COPY apps/backend/ .

ENV MEDUSA_DISABLE_MEDUSA_ADMIN=true
RUN npm run build

ENV NODE_ENV=production
ENV MEDUSA_DISABLE_MEDUSA_ADMIN=true
ENV NODE_OPTIONS=--max-old-space-size=512

EXPOSE 9000

CMD ["sh", "-c", "node_modules/.bin/medusa db:migrate && node .medusa/server/index.js"]
