# 1. Install dependencies
FROM node:18.12.1-alpine AS dependencies
# Add libc6-compat package https://github.com/nodejs/docker-node/tree/b4117f9333da4138b03a546ec926ef50a31506c3#nodealpine
RUN apk add --no-cache libc6-compat

WORKDIR /app

COPY package.json package-lock.json ./

RUN \
    if [ -f package-lock.json ]; then npm ci; \
    else echo "Lockfile not found." && exit 1; \
    fi

# 2. Start the development server
FROM node:18.12.1-alpine AS development-server

WORKDIR /app

COPY --from=dependencies --chown=node:node /app/node_modules ./node_modules
COPY --chown=node:node . .

EXPOSE 3000

USER node

CMD ["npm", "run", "dev"]
