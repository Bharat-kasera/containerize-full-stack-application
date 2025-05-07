### 1. docker-compose.yml

```yaml
name: "fullstack-app"

services:
  redis:
    image: redis:7-alpine
    command: ["redis-server", "--requirepass", "Test#123"]
    ports:
      - "6380:6380"
    volumes:
      - redis:/data
    networks:
      - my-app-network

  mongo:

    image: mongo:4.4
    environment:
      MONGO_INITDB_ROOT_USERNAME: ${MONGO_INITDB_ROOT_USERNAME}
      MONGO_INITDB_ROOT_PASSWORD: ${MONGO_INITDB_ROOT_PASSWORD}
    ports:
      - "27017:27017"
    volumes:
      - mongo:/data/db
    networks:
      - my-app-network

  backend:
    container_name: backend
    build:
      context: ./backend
    env_file:
      - ./backend/.env
    ports:
      - "8080:8080"
    volumes:
      - ./backend:/app
      - /app/node_modules
    depends_on:
      - redis
      - mongo
    networks:
      - my-app-network
    command: sh -c "sleep 5 && node seed.js"

  frontend:
    container_name: frontend
    build:
      context: ./frontend
    env_file:
      - ./frontend/.env
    ports:
      - "5173:5173"
    volumes:
      - ./frontend:/app
      - /app/node_modules
    depends_on:
      - backend
    networks:
      - my-app-network

volumes:
  redis:
  mongo:

networks:
  my-app-network:
    driver: bridge

```

### 2. Backend Dockerfile (backend/Dockerfile)

```Dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 8080

CMD ["npm", "run", "dev"] 
```

### 3. Frontend Dockerfile (frontend/Dockerfile)

```Dockerfile
FROM node:22

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

CMD ["npm", "run", "dev",  "--", "--host"]
```

