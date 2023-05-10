# Working-with-Docker

## Demo App 

This demo app shows a simple user profile app set up using:

- `index.html` with pure JS and CSS styles
- Node.js backend with Express module
- MongoDB for data storage

All components are Docker-based.

## Use Docker for local development

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

## Docker Compose - Run multiple Docker containers

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

## Dockerize Nodejs application and push to private Docker registry:

1. Install AWS-CLI.
2. Configure AWS-CLI with your AWS credentials.
3. Create an ECR repo named "my-app".
4. Execute the following commands:
   1. Retrieve an authentication token and authenticate your Docker client to your registry. Use the AWS CLI:

      ```shell
      aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin 732909161122.dkr.ecr.eu-north-1.amazonaws.com
      ```

      *Note: If you receive an error using the AWS CLI, make sure that you have the latest version of the AWS CLI and Docker installed.*

   2. Build your Docker image using the following command. For information on building a Docker file from scratch, see the instructions [here](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/docker-basics.html). You can skip this step if your image is already built:

      ```shell
      docker build -t my-app .
      ```

   3. After the build completes, tag your image so you can push the image to this repository:

      ```shell
      docker tag my-app:latest 732909161122.dkr.ecr.eu-north-1.amazonaws.com/my-app:latest
      ```

   4. Run the following command to push this image to your newly created AWS repository:

      ```shell
      docker push 732909161122.dkr.ecr.eu-north-1.amazonaws.com/my-app:latest
      ```

## Deploy Docker application on a server with Docker Compose:

1. Add the ECR repository URL to the YAML file and open port 3000.
2. Copy the YAML file to the dev server.
3. Retrieve an authentication token and authenticate your Docker client to your registry. Use the AWS CLI:

   ```shell
   aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin 732909161122.dkr.ecr.eu-north-1.amazonaws.com
   ```

4. Run the following command:

   ```shell
   docker-compose -f docker-compose.yaml up
   ```

## Persist data with Docker Volumes:

1. Attach the volume in the `docker-compose` file.
2. To find the volumes, use the command:

   ```shell
   ls /var/lib/docker/volumes
   ```

---


