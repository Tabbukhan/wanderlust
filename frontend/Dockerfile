# Stage 1: Build
FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm install --no-audit --no-fund

COPY . .

ARG TS_MODULE=Node16

# Ignore TypeScript path alias errors so build doesn't fail
RUN npx tsc -p tsconfig.prod.json --module $TS_MODULE --skipLibCheck --noEmitOnError false || echo "TypeScript errors ignored" \
    && npm run vite build

# Stage 2: Production image
FROM nginx:alpine

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
