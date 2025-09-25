# Stage 1: Build
FROM node:20-alpine AS build

WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install --no-audit --no-fund

# Copy the rest of the code
COPY . .

# Set TS module resolution for Node16 without changing repo
ARG TS_MODULE=Node16

# Build with TS module override
RUN npx tsc -p tsconfig.prod.json --module $TS_MODULE --skipLibCheck --noEmitOnError false \
    && npm run vite build

# Stage 2: Production
FROM nginx:alpine AS prod

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
