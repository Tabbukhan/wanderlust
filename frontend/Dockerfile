# Build stage
FROM node:20-alpine AS build

# Allow module resolution for Node16 to fix TS error
ARG TS_MODULE=Node16

WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install --no-audit --no-fund

# Copy source code
COPY . .

# Build TypeScript + Vite project
RUN npx tsc -p tsconfig.prod.json --module $TS_MODULE && npm run vite build

# Production stage
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
