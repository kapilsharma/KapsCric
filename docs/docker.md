# Docker setup

- Make a new folder, say `KapsCricDev`
- Within that folder, make a file docker-compose.yml and add following cod in it

```yaml
version: '3.8'

services:
  backend:
    build:
      context: ./kapscric-backend
      dockerfile: Dockerfile
    ports:
      - "3000:3000" # API and WebSocket
    environment:
      - NODE_ENV=development
      - MONGO_URI=mongodb://mongo:27017/kapscric
      - JWT_SECRET=your_jwt_secret_here
      - EMAIL_HOST=smtp.example.com # Replace with AWS SES or other SMTP
      - EMAIL_USER=your_email_user
      - EMAIL_PASS=your_email_pass
    depends_on:
      - mongo
    volumes:
      - ./kapscric-backend:/app
      - /app/node_modules
    networks:
      - kapscric-net

  mongo:
    image: mongo:6.0
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
    networks:
      - kapscric-net

  frontend:
    build:
      context: ./kapscric-frontend
      dockerfile: Dockerfile
    ports:
      - "5173:5173" # Vite dev server
    environment:
      - VITE_API_URL=http://localhost:3000
    volumes:
      - ./kapscric-frontend:/app
      - /app/node_modules
    networks:
      - kapscric-net

networks:
  kapscric-net:
    driver: bridge

volumes:
  mongo-data:
```

## Yaml

Yaml is also a structure like Json. It can be compare like objects and array

At top level, we make key-value paid like

```yaml
key1: value1
key2: value2
```

These value can further contains following value

- String
- Integer
- Boolean
- object (key-value)
- array (multiple values without key)

### Object values

```yaml
build:
  context: ./kapscric-backend
  dockerfile: Dockerfile
```

In above example, value of `build` key is aagain an object with two keys. We define it by two spaces. In Yaml, we define children by spaces

### Array value

Array is a collection of data. Example

```yaml
volumes:
  - ./kapscric-backend:/app
  - /app/node_modules
```

Here, volumes is an array with two values. We define it by `-`.

## Understanding docker-compose file

Now we have a quick idea of yaml, we can go ahead to understand the structure and meaning of out docker-compose.yaml file. At a root level, it have 4 keys

- version
- services
- networks
- volumes

### Version

It simple define the version of file format. Let's not go into much of its details (That's not the docker notes), just understand, we should keep it at 3.8.

### Services

This is where we define our services. It is again an object and there are three keys:

- backend
- mongo
- frontend

You might have remember from out [TDD](./tdd.md) that we defined three layers for frontend (React), backend (NodeJS/Express), and Database (MongoDB). We are just defining them here.

#### Backend

It is again an object with following details

```Yaml
backend:
  build:
    context: ./kapscric-backend
    dockerfile: Dockerfile
  ports:
    - "3000:3000" # API and WebSocket
  environment:
    - NODE_ENV=development
    - MONGO_URI=mongodb://mongo:27017/kapscric
    - JWT_SECRET=your_jwt_secret_here
    - EMAIL_HOST=smtp.example.com # Replace with AWS SES or other SMTP
    - EMAIL_USER=your_email_user
    - EMAIL_PASS=your_email_pass
  depends_on:
    - mongo
  volumes:
    - ./kapscric-backend:/app
    - /app/node_modules
  networks:
    - kapscric-net
```

- build: It defines how we want to make our server. We can get any existing image (more on it later) or make our own image. Here, we choose to make our own image.
  - Consider image is Operating system installer CD, which is used to install OS on your system. We use image (CD) to install OS and some programs on our Container (Consider it as server/computer)
  - `context` says where our files are present to make the image
  - `dockerfile` tell path of dockerfilw, used to make the image. We will create these folder and files soon.
- `ports`. Once we have our container (server) ready, we want to tell which ports we want to expose
  - Ports are the address of physical connectors on server hardware. For security purpose, all ports on the server are closed by default. We need to manually configure which ports we want to open.
  - Since we may want to open multiple ports, this is an array.
  - Here, we just have one value in ports `3000:3000`.
  - It says, port 3000 on docker container should be open and it should be available as port 3000 on our host machine (Your laptop/computer)
- `environment`
  - This represent environmental variables on the server.
  - For security purpose, we might not want to commit confidential information like passwords, keys, etc of our stage/live servers on git. One of the ways is to save them as environmental variables on the server. There are multiple other ways,
  - Don't worry for it right now as this is Dev-ops thing but we need to understand all the fields given there are secret information
- `depends_on`. This define if our server (container) depends on any other container. Our backend server depends on database server (Mongo DB) and we defined it here.
- `volumes`. Consider volumes as harddisk space. One we close our docker server, all the data will be erased. We might want to keep some of the data between the restart of the servers. Thus, we define volumes, as some storage outside our container, which will not be lost once container shut down. More on it later.
- `network`. Last but not least, we have a network, over which our containers will communicate with each other. We have defined out own custom network `kapscric-net` (defined below in Yaml file) and using that.

#### Mongo db

We defined container for Mongo as well

```yaml
mongo:
    image: mongo:6.0
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
    networks:
      - kapscric-net
```

- image: As told in previous section, we can either build the image or reuse any existing image. In backend, we made our own image but here, we are reusing image provided by MongoDB directly.
- port. volume, networks are same as earlier.

#### frontend

It is also defined same as backend, we just have some different ports and environment.

### Networks

Here, we are just defineing a custom network with bridge driver. Without going into lot of details, just consider, it is something our containers need to connect.


### Volumes

As discussed in backend sections, we setup a disk space on host machine (your computer) so that when we restart servers, data is not lost. Here we need to define a volume to persist data in the Mongo database.

# Docker prerequisite

I assume you already have docker installed on your server. If no, please do some google search and install it. I'll not go into docker installation but quickly jump to Node and react code.

# Docker setup continued

We currently have a folder `KapsCricDev` which contains `docker-compose.yml` file in it.

If we want to setup from scratch, we can create two new folders within `KapsCricDev`

- kapscric-backend
- kapscric-frontend

Alternately, we can also checkout them from Github

```bash
git clone git@github.com:kapilsharma/kapscric-backend.git
git clone git@github.com:kapilsharma/kapscric-frontend.git
```

## Creating Dockerfile

If you remember, we are building our own image for frontend and backend and mentioned Dockerfile to create them. Let's create docker files there

**kapscric-backend/Dockerfile**

```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "run", "dev"]
```

**kapscric-frontend/Dockerfile**

```Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5173
CMD ["npm", "run", "dev"]
```

Code in both the files is exactly the same, except the port. Let's quickly go through what we are doing here

- `FROM`: We are making our own docker image but we never make it from scratch. This is one of the best part of docker. We can take a base image and define our configuration there.
  - We are taking our base image as `node:18-alpine`.
  - It is an image provided by Node with version 18 installed on Alpine linux. Alpine is a minimal and small linux. We use light weight linux as we do not need everything but just nodejs running there. If we take ubuntu or other popular linux, its container size will be huge.
- `WORKDIR /app`. I guess it is clear. We are saying our work directory is `/app`, where we will keep our code.
- `COPY package*.json ./`. This is simple copy command. Once we setup our project, all instructions on npm packages are there in package.json and package-lock.json. We are simply copying those files from our host machine (your computer) to the container
- `RUN npm install`. We use `RUN` to execute any command. We are simply executing `npm install` to install the dependencies
- `COPY . .`: Here we are copying all the files from git repo on host machine to the docker container in WORKDIR
- `EXPOSE 3000`: Exposing port 3000
- `CMD ["npm", "run", "dev"]`: At the end, we are running `npm run dev` command.

**Difference between RUN and CMD**

One of the common question of many developers is, what is the difference between `RUN` and `CMD` command, as they both are running command on terminal.

Difference is the Docker lifecycle

- `RUN` runs the command only once, while creating the image
- `CMD` runs command everytime, docker container starts.

We want to run `npm install` only once (later if needed, we will do it manually). However, we want to start the dev server and execire `npm run dev` everytime we start the docker container.

## Next step

This makes the basic setup. We can run `docker-compose up --build` to start our docker containers.

However, if you are making it from scratch, you will get errors in our backend and frontend containers. To start with, it will complain no `package.json` file found, which is true.

Let's continue our setup in Backend and frontend.

We will first start with backend. For that, please refer README.md file in backend repository.
