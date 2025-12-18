FROM node:22-alpine AS builder

WORKDIR /app

COPY src/postgres /app
COPY tsconfig.json /app/tsconfig.json
COPY package.json /app/package.json
COPY package-lock.json /app/package-lock.json

RUN npm ci
RUN npm run build

FROM node:22-alpine AS release

WORKDIR /app

COPY --from=builder /app/dist /app/dist
COPY --from=builder /app/package.json /app/package.json
COPY --from=builder /app/package-lock.json /app/package-lock.json

ENV NODE_ENV=production

RUN npm ci --omit-dev

CMD ["sh", "-c", "node dist/index.js \"$DATABASE_URL\""]