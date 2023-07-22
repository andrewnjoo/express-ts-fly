## Instructions from scratch

Create a new directory, initialize npm, and install dependencies:

```
mkdir express-typescript
cd express-typescript
npm init -y
npm install express typescript ts-node @types/node @types/express

```

Create a tsconfig.json file with the following:

```json
{
  "compilerOptions": {
    "target": "ES2019",
    "module": "commonjs",
    "outDir": "dist",
    "rootDir": "src",
    "strict": true,
    "esModuleInterop": true
  }
}
```

* `--target` means which version of ECMAScript you're using to code. `--module` means which module system you're using such as commonjs for Node or ES module for all that supports it and what not. [1](https://stackoverflow.com/questions/39493003/typescript-compile-options-module-vs-target)


Creat a src folder:

```bash
mkdir src
```

Create an express file:

```bash
touch src/index.ts
```

and add the following code:

```typescript
import express from "express";

const app = express();

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(3000, () => {
  console.log("Server running on port 3000");
});
```

Add the following scripts to `package.json`:

```json
{
  "scripts": {
    "start": "node dist/index.js",
    "dev": "nodemon src/index.ts",
    "build": "tsc"
  }
}
```

Create a Dockerfile:

```Dockerfile
# Use the official Node.js 14 image as the base image
FROM node:14

# Set the working directory inside the container
WORKDIR /usr/src/app

# Copy the package.json and package-lock.json files to the container
COPY package*.json ./

# Install project dependencies
RUN npm install

# Copy the rest of the application code to the container
COPY . .

# Build the TypeScript code
RUN npm run build

# Expose the port the app will run on
EXPOSE 3000

# Start the Express server
CMD ["npm", "start"]
```

Then launch the app with fly (follow the prompts):

```
fly launch
```

Deploy the app with:

```
fly deploy
```

Make sure to install fly first with `brew install flyctl`

Then go to [fly](https://fly.io/dashboard), to see your app running!

