FROM node:24-alpine

# v1.0.0
LABEL maintainer="muah1987 <muah1987@users.noreply.github.com>" \
      author="muah1987" \
      version="1.0.0" \
      description="Custom MCP server for Ollama (by muah1987)" \
      license="GPL-2.0"

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy server code
COPY server.js ./
COPY src/ ./src/

# Make server executable
RUN chmod +x server.js

# Use non-root user
USER node

# Start the MCP server
CMD ["node", "server.js"]