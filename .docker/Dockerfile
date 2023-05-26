ARG NODE_ENV

# ベースステージ
FROM node:18 as base

ENV NODE_ENV=${NODE_ENV}
WORKDIR /home/node/workspace
RUN chown node:node . && \
    mkdir node_modules && \
    chown node:node node_modules

USER node:node

# 開発用ステージ
FROM base as development

COPY --chown=node:node package.json yarn.lock ./
RUN yarn global add nuxi nuxt3 && \
    yarn install && \
    git config --global --add safe.directory /home/node/workspace

ENTRYPOINT ["yarn", "dev"]

# ビルダーステージ
FROM base as builder

COPY --chown=node:node package.json yarn.lock ./
RUN yarn install
COPY --chown=node:node . ./
RUN yarn build

# 本番用ステージ
FROM gcr.io/distroless/nodejs18-debian11:nonroot as production

WORKDIR /app
USER nonroot

COPY --chown=nonroot:nonroot --from=builder /home/node/workspace/.output /app

CMD ["./server/index.mjs"]
