# Stage 1: Build
FROM node:20-alpine AS build

WORKDIR /app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install --no-audit --no-fund

# Copy all source files
COPY . .

# Build argument for TS module
ARG TS_MODULE=Node16

# Build the project
RUN npx tsc -p tsconfig.prod.json --module $TS_MODULE --skipLibCheck --noEmitOnError false \
    && npm run vite build

# Stage 2: Production image
FROM nginx:alpine

# Copy built files to nginx html folder
COPY --from=build /app/dist /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
