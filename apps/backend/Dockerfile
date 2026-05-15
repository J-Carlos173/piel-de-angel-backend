FROM node:20-alpine

WORKDIR /app

COPY package.json package-lock.json ./
COPY apps/backend/package.json ./apps/backend/

RUN npm install --legacy-peer-deps --include=dev

COPY apps/backend/ ./apps/backend/

WORKDIR /app/apps/backend

ENV MEDUSA_DISABLE_MEDUSA_ADMIN=false
RUN NODE_OPTIONS=--max-old-space-size=2048 /app/node_modules/.bin/medusa build

ENV NODE_ENV=production
ENV NODE_OPTIONS=--max-old-space-size=768

EXPOSE 9000

CMD ["sh", "-c", "/app/node_modules/.bin/medusa db:migrate && /app/node_modules/.bin/medusa user -e ${MEDUSA_ADMIN_EMAIL} -p ${MEDUSA_ADMIN_PASSWORD} 2>/dev/null || true && /app/node_modules/.bin/medusa start"]
