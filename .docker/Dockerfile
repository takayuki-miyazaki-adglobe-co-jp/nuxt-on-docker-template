ARG NODE_ENV

# ベースステージ
FROM node:18 as base

ENV NODE_ENV=${NODE_ENV}
WORKDIR /home/node/workspace
RUN mkdir node_modules && \
    chown node:node -R .

# 開発用ステージ
FROM base as development

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y locales && \
    echo "ja_JP.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen ja_JP.UTF-8 && \
    dpkg-reconfigure locales && \
    /usr/sbin/update-locale LANG=ja_JP.UTF-8

ENV LC_ALL=ja_JP.UTF-8

USER node:node

COPY --chown=node:node package.json yarn.lock ./
RUN yarn global add nuxi nuxt3 && \
    yarn install

ENTRYPOINT ["yarn", "dev"]

# ビルダーステージ
FROM base as builder

USER node:node

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
