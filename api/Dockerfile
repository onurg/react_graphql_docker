FROM node:8 as build

ARG NPM_TOKEN
ARG APP_DIR=/opt/app

WORKDIR $APP_DIR

# create a layer containing the node modules
COPY package.json package-lock.json $APP_DIR/
RUN npm ci

COPY . $APP_DIR

RUN rm -rf node_modules && \
  npm ci --production

FROM node:8

ARG APP_DIR=/opt/app
WORKDIR $APP_DIR

COPY --from=build $APP_DIR/node_modules $APP_DIR/node_modules
COPY --from=build $APP_DIR/package.json $APP_DIR/package.json

# get the build number at build time, and set it
# as an environment variable for access at run time
ARG BUILD_NUMBER
ENV BUILD_NUMBER ${BUILD_NUMBER}

# get the commit hash at build time, and set it
# as an environment variable for access at run time
ARG COMMIT_HASH
ENV COMMIT_HASH ${COMMIT_HASH}

EXPOSE 3030

USER node

ENTRYPOINT ["node", "api.js"]
