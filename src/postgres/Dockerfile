FROM node:22-alpine AS builder

WORKDIR /app

COPY src/postgres /app
COPY src/postgres/tsconfig.json /app/tsconfig.json
COPY src/postgres/package.json /app/package.json

RUN npm ci
RUN npx tsc

FROM node:22-alpine AS release

WORKDIR /app

COPY --from=builder /app/dist /app/dist
COPY --from=builder /app/package.json /app/package.json
COPY --from=builder /app/package-lock.json /app/package-lock.json

ENV NODE_ENV=production

RUN npm ci --omit-dev

CMD ["sh", "-c", "echo \"Starting MCP Postgres\" && echo \"DATABASE_URL=$DATABASE_URL\" && node dist/index.js \"$DATABASE_URL\""]
