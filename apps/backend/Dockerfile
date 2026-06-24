FROM node:20-alpine AS prepare
WORKDIR /app
RUN npm install -g turbo
COPY . .
RUN turbo prune backend --docker

FROM node:20-alpine AS builder
WORKDIR /app
COPY --from=prepare /app/out/json .
RUN npm install
COPY --from=prepare /app/out/full .
RUN npm run build

FROM node:20-alpine AS runner
WORKDIR /app
COPY --from=builder /app/apps/backend/dist ./dist/
COPY --from=builder /app/apps/backend/package.json ./
COPY --from=builder /app/node_modules ./node_modules
EXPOSE 3000
CMD ["node", "/app/dist/src/main.js"]


