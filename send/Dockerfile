# installing dependencies
FROM node:20-alpine3.17 as deps
WORKDIR /app
COPY package.json /app
COPY package-lock.json /app
RUN npm install

# building the application
FROM node:20-alpine3.17 as builder
WORKDIR /app
COPY --from=deps /app/node_modules node_modules
COPY . /app
RUN npm run build

# running our application
FROM node:20-alpine3.17 as runner
WORKDIR /app
COPY --from=builder /app/.next/standalone ./standalone
COPY --from=builder /app/public ./standalone/public
COPY --from=builder /app/.next/static ./standalone/.next/static
EXPOSE 3000
RUN adduser -S nextjs -u 1001
USER nextjs
CMD ["node", "./standalone/server.js"]
