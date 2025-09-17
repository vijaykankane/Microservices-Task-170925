# Multi-stage build for production optimization
FROM node:18-alpine AS base

# Install security updates, dumb-init, and wget for health checks
RUN apk update && apk upgrade && \
    apk add --no-cache dumb-init wget && \
    rm -rf /var/cache/apk/*

# Create non-root user for security
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodeuser -u 1001

FROM base AS deps
WORKDIR /app
# Copy package files for dependency installation
COPY package*.json ./
# Install dependencies (using npm install since no package-lock.json exists)
RUN npm install --omit=dev && npm cache clean --force

FROM base AS runtime
WORKDIR /app

# Copy dependencies from deps stage
COPY --from=deps --chown=nodeuser:nodejs /app/node_modules ./node_modules
# Copy application source
COPY --chown=nodeuser:nodejs . .

# Switch to non-root user
USER nodeuser

# Expose port
EXPOSE 3003


# Use dumb-init for proper signal handling
ENTRYPOINT ["dumb-init", "--"]
CMD ["node", "app.js"]