
FROM node:22

WORKDIR /app

# Install dependencies
COPY frontend/package*.json ./
RUN npm install

# Copy source code & build
COPY frontend/. .
RUN npm run build

EXPOSE 3000

CMD ["npm", "run", "dev"]
