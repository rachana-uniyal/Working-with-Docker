# Working-with-Docker

Here's the given text converted to Markdown:

---

# Demo App - Developing with Docker

This demo app shows a simple user profile app set up using:

- `index.html` with pure JS and CSS styles
- Node.js backend with Express module
- MongoDB for data storage

All components are Docker-based.

## With Docker

### To start the application

**Step 1: Create Docker network**

```shell
docker network create mongo-network
```

**Step 2: Start MongoDB**

```shell
docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo
```

**Step 3: Start Mongo Express**

```shell
docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express
```

*Note: Creating a Docker network is optional. You can start both containers in a default network. In this case, just omit the `--net` flag in the Docker run command.*

**Step 4: Open Mongo Express from a browser**

[http://localhost:8081](http://localhost:8081)

**Step 5: Create `user-account` database and `users` collection in Mongo Express**

**Step 6: Start your Node.js application locally - go to the app directory of the project**

```shell
cd app
npm install
node server.js
```

**Step 7: Access your Node.js application UI from a browser**

[http://localhost:3000](http://localhost:3000)

## With Docker Compose

### To start the application

**Step 1: Start MongoDB and Mongo Express**

```shell
docker-compose -f docker-compose.yaml up
```

You can access Mongo Express at [http://localhost:8080](http://localhost:8080) from your browser.

**Step 2: In Mongo Express UI - Create a new database named "my-db"**

**Step 3: In Mongo Express UI - Create a new collection named "users" in the "my-db" database**

**Step 4: Start Node server**

```shell
cd app
npm install
node server.js
```

**Step 5: Access the Node.js application from a browser**

[http://localhost:3000](http://localhost:3000)

### To build a Docker image from the application

```shell
docker build -t my-app:1.0 .
```

The dot `.` at the end of the command denotes the location of the Dockerfile.

---

Please note that Markdown does not support the exact formatting of code blocks in different shells, so the above formatting is a general representation.
