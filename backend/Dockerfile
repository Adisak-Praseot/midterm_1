# Stage 1: Build
FROM node:20-alpine AS builder

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install all dependencies (including devDependencies if needed)
RUN npm ci

# Copy application code
COPY . .

# Stage 2: Production
FROM node:20-alpine

WORKDIR /app

# Set Node environment to production
ENV NODE_ENV=production

# Create logs directory for volume mount
RUN mkdir -p logs

# Copy package files from builder
COPY package*.json ./

# Install only production dependencies
RUN npm ci --only=production

# Copy application code from builder stage
COPY --from=builder /app /app

# Healthcheck - Verify service is responding
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost:3000/api/demo || exit 1

# Expose port
EXPOSE 3000

# Start application
CMD ["node", "server.js"]
