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

CMD ["sh", "-c", "/app/node_modules/.bin/medusa db:migrate && /app/node_modules/.bin/medusa start"]
