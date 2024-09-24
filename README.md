# How to create and deploy a React Vite App using Docker

https://thedkpatel.medium.com/dockerizing-react-application-built-with-vite-a-simple-guide-4c41eb09defa

1. How to quickly create a React App using Vite:

   ```
   npm create vite@latest docker-react-app-test --template react-ts
   ```

2. When setting up a vite react app with Vite, should I select the `TypeScript` variant or the `TypeScript + SWC` variant? Answer: `Typescript`

3. Install Docker: https://docs.docker.com/engine/install/

4. Setup `Dockerfile`:

   ```
   FROM node:20

   WORKDIR /app

   COPY ./package*.json ./

   RUN npm install

   COPY . .

   RUN npm run build

   EXPOSE 8080

   CMD [ "npm", "run", "preview", "--", "--host" ]
   ```

5. Update `vite.config.ts`:

   ```
   import { defineConfig } from "vite";
   import react from "@vitejs/plugin-react";

   export default defineConfig({
   base: "/",
   plugins: [react()],
   preview: {
       port: 8080,
       strictPort: true,
   },
   server: {
       port: 8080,
       strictPort: true,
       host: true,
       origin: "http://0.0.0.0:8080",
   },
   });
   ```

6. Run two commands:

   1. `docker build -t docker-react-app-test .`
   2. `docker run -d -p 8080:8080 docker-react-app-test`

7. Open app at `localhost:8080`
