# Base image
FROM node:14 as base

WORKDIR /usr/app

# Dependencies
COPY package*.json ./
RUN npm install

# Build image
FROM base as build

WORKDIR /usr/app

# Build
COPY . ./
RUN npm run build

# production image
# FROM node:17.0.0-alpine as production
FROM alpine:3.13 as production

# Dependencies
RUN apk add --update nodejs npm && rm -rf /var/cache/apk/*

# Create user
RUN addgroup -S node && adduser -S node -G node
USER node
ENV NODE_ENV=production

# Copy app
WORKDIR /usr/app
COPY --from=base /usr/app/package.json ./
RUN npm install --only=production
COPY --from=build /usr/app/dist ./

# Application
ENV PORT=8080
EXPOSE 8080
CMD ["node", "main.js"]
