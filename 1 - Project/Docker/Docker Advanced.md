### Layering
Docker can be thought of a series of layers, with each next layer building on top of the previous layer. Docker image is built incrementally, first base image, then layer1, then layer2, ... until the final layer.

Properties of layers
- Layers are reused across multiple images, speeding up build and download times
- Layers are immutable, once created cannot be changed. If there is a change a new layer is created to replace the old layer.

Consequence of immutability
- If a layer is cached, all layers below it must be cached.
- If one layer is uncached, all layers after it are un-cached.

```Dockerfile
FROM node:20
WORKDIR /app

# Caching node_modules and prisma, until package.json changed
COPY package* .
COPY ./prisma .

RUN npm install
RUN npx prisma generate

# Copy remaining src and build the rest
COPY . .
RUN npm run build
EXPOSE 3000

CMD ["node", "index.js"]
```


Case 1:
- Only src code changes, package.json code remains constant, in that case cached till `COPY . .`
- package.json changes, Only cached till `COPY package* .`
