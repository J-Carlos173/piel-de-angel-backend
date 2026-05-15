FROM node:20-alpine

WORKDIR /app

COPY package.json package-lock.json ./
COPY apps/backend/package.json ./apps/backend/

RUN npm install --legacy-peer-deps --include=dev

COPY apps/backend/ ./apps/backend/

WORKDIR /app/apps/backend

ENV MEDUSA_DISABLE_MEDUSA_ADMIN=false
RUN NODE_OPTIONS=--max-old-space-size=4096 /app/node_modules/.bin/medusa build

# medusa build puts admin at .medusa/server/public/admin/
# but medusa start (cwd=/app/apps/backend) looks for it at public/admin/
RUN if [ -d ".medusa/server/public/admin" ]; then \
      echo "Admin build found — copying to public/admin/" && \
      mkdir -p public/admin && \
      cp -r .medusa/server/public/admin/. public/admin/; \
    else \
      echo "ERROR: Admin build not found at .medusa/server/public/admin" && \
      ls -la .medusa/ && exit 1; \
    fi

ENV NODE_ENV=production
ENV NODE_OPTIONS=--max-old-space-size=4096

EXPOSE 9000

CMD ["sh", "-c", "/app/node_modules/.bin/medusa db:migrate && /app/node_modules/.bin/medusa user -e ${MEDUSA_ADMIN_EMAIL} -p ${MEDUSA_ADMIN_PASSWORD} 2>/dev/null || true && /app/node_modules/.bin/medusa start"]
