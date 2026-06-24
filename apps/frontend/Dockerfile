FROM node:20-alpine AS prepare
WORKDIR /app
RUN npm install -g turbo
COPY . .
RUN turbo prune web --docker


FROM node:20-alpine AS builder
WORKDIR /app
COPY --from=prepare /app/out/json/ .
RUN npm install
COPY --from=prepare /app/out/full/ .
RUN npm run build

FROM node:20-alpine AS runner
WORKDIR /app
COPY --from=builder /app/apps/frontend/.next/ ./.next/
COPY --from=builder /app/apps/frontend/package.json ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/apps/frontend/public ./public
EXPOSE 3001
CMD ["npm", "run", "start"]
