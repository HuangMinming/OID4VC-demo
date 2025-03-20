# Builder stage
FROM node:lts-bookworm AS oid4vc-demo-builder
SHELL ["/bin/bash", "-c"]
RUN npm -g install pnpm && SHELL=bash pnpm setup && source /root/.bashrc && pnpm self-update
RUN pnpm add typescript tslib sqlite3
WORKDIR /opt/oid4vc-demo

COPY /. .
RUN rm -rf /root/.local/share/pnpm/store
RUN pnpm install -r
RUN pnpm build

# Final stage
FROM node:lts-bookworm
SHELL ["/bin/bash", "-c"]
RUN npm -g install pnpm && SHELL=bash pnpm setup && source /root/.bashrc && pnpm self-update
RUN pnpm add typescript tslib
RUN pnpm add sqlite3 --save
RUN pnpm list
WORKDIR /opt/oid4vc-demo

COPY /. .
RUN pnpm install -r
RUN pnpm build
ARG PACKAGE_PATH
ARG NODE_SCRIPT
ENV NODE_SCRIPT=${NODE_SCRIPT}
#COPY --from=oid4vc-demo-builder /opt/oid4vc-demo/${PACKAGE_PATH} /opt/oid4vc-demo/${PACKAGE_PATH}
# package node_modules has symlinks to root node_modules
#COPY --from=oid4vc-demo-builder /opt/oid4vc-demo/node_modules /opt/oid4vc-demo/node_modules

WORKDIR /opt/oid4vc-demo/${PACKAGE_PATH}
# ENTRYPOINT npm run-script ${NODE_SCRIPT}
