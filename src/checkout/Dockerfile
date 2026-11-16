FROM node:24-slim AS builder

WORKDIR /app

RUN npm install -g corepack && corepack enable
COPY package.json yarn.lock .yarnrc.yml ./
COPY .yarn ./.yarn
RUN yarn install --immutable
COPY . .

RUN yarn build
RUN yarn workspaces focus --production

FROM node:24-slim

WORKDIR /app
ENV NODE_ENV=production

COPY --from=builder --chown=node:node /app/package.json /app/
COPY --from=builder --chown=node:node /app/node_modules /app/node_modules
COPY --from=builder --chown=node:node /app/dist /app/dist
USER node

EXPOSE 8080

CMD ["node","dist/main"]