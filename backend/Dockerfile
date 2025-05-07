
FROM node:22

WORKDIR /app

# Install dependencies
COPY backend/package*.json ./
RUN npm install

# Copy source code
COPY backend/. .

# Expose port
EXPOSE 8080

# Start the app
CMD ["npm", "run","dev"]
