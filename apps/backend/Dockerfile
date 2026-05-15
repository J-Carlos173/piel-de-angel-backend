FROM node:20-alpine

WORKDIR /app

# Copy root workspace manifests
COPY package.json package-lock.json ./

# Copy backend package.json so npm recognizes the workspace
COPY apps/backend/package.json ./apps/backend/

# Install all dependencies from workspace root
RUN npm install --legacy-peer-deps --include=dev

# Copy backend source
COPY apps/backend/ ./apps/backend/

# Build the backend
WORKDIR /app/apps/backend

RUN /app/node_modules/.bin/medusa build

ENV NODE_ENV=production
ENV NODE_OPTIONS=--max-old-space-size=512

EXPOSE 9000

ENV MEDUSA_DISABLE_MEDUSA_ADMIN=true

CMD ["sh", "-c", "/app/node_modules/.bin/medusa db:migrate && /app/node_modules/.bin/medusa user -e ${MEDUSA_ADMIN_EMAIL} -p ${MEDUSA_ADMIN_PASSWORD} 2>/dev/null || true && /app/node_modules/.bin/medusa start"]
