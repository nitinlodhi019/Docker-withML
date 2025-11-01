# Docker

Docker is a platform that allows you to package, ship, and run applications in isolated, lightweight environments called containers.

<img width="659" height="440" alt="image" src="https://github.com/user-attachments/assets/1060b331-dfe3-4e4c-b98c-03e4ec95550e" />


Docker is a tool that creates and runs containers — mini-environments that contain:

* Your application code
* Required libraries & dependencies
* Runtime (Python, Node, etc.)
* OS-level files

Think of a Docker container like a self-contained box that has everything your app needs to run, no matter where it runs.

## without Docker

Your app works on your machine, but fails on someone else’s:

“It works on my system 🤷‍♂️ but not on the server!”

Why?

Different systems have different:

* Python versions
* Libraries
* OS dependencies
* Configurations

## With Docker

You ship everything together → It works everywhere.

"If it runs in my container, it runs anywhere."

## Why Docker Is Needed

**1. Consistency**

Without Docker: Environment differs → app breaks

With Docker: Same environment everywhere.

**2. Dependency Isolation**

You can have 2 FastAPI apps:
```
App        Python        Libraries
App A	     3.8	        Pandas 1.3
App B	     3.11	        Pandas 2.2
```
Normally impossible on one system without virtualenv headaches.

In Docker? Both coexist ✅
They live in separate containers.

**3. Fast Deployment**

**4️. Scalability (Cloud + Kubernetes)**

Companies deploy thousands of containers daily.
Docker + Kubernetes is industry standard.

Netflix, Google, Uber, Amazon → all use containers.

**5. Security & Isolation**

Each app runs isolated.
If one crashes or gets hacked, others are safe.

**6. Microservices**

Modern systems use microservices:

* auth service

* ML model service

* database

* logging

* notification system

Each runs in its own container. Independent. Scalable.

**7️. Machine Learning Benefit**

ML models require exact dependencies:

* NumPy version

* TensorFlow/PyTorch version

* CUDA for GPU

* OS compatibility

Docker freezes the environment →
model runs on any machine, always same.

**8️. No OS conflict**

Windows app runs on Linux server via Docker.

**9️. Teams & DevOps**

In industry:

* Developer writes code

* DevOps deploys it

* QA tests it

Docker guarantees same environment for all.

## How exactly docker is used??

<img width="1000" height="546" alt="image" src="https://github.com/user-attachments/assets/4c058c8a-b914-4d61-86e4-7b59263bd21b" />

## Docker Engine

Docker Engine is an open-source client-server technology that is the core component of the Docker platform, responsible for building, running, and managing containerized applications. It enables applications to run consistently across different environments (desktops, data centers, cloud providers), solving "dependency hell" for developers. 

<img width="800" height="517" alt="image" src="https://github.com/user-attachments/assets/abcee66a-57d7-4ca9-a58b-9962a3e25cff" />

### Components of Docker Engine

* **Docker Daemon (dockerd):** This is a persistent background process that runs on the host machine. It manages Docker objects such as images, containers, networks, and volumes. The daemon listens for Docker API requests and performs the heavy lifting of creating and managing containers. It can also communicate with other daemons on the same or different host machines.
* **REST API:** This provides a programmatic interface that allows applications and tools to interact with the Docker Daemon. It enables external tools and the Docker CLI to send commands and retrieve information from the daemon.
* **Docker CLI (Command Line Interface):** This is the user-facing tool (the docker command) that allows users to interact with the Docker Daemon. When you type commands like docker run, docker build, or docker pull, the CLI translates these commands into API requests and sends them to the Docker Daemon.

## Docker Image

A Docker image is a lightweight, standalone, executable software package that includes everything needed to run an application. This includes the application code, a runtime, system tools, libraries, and settings. Essentially, a Docker image acts as a read-only template or blueprint for creating Docker containers. 

### Components of a Docker image

* **Base Image:** This is the foundation of your image, often a minimal operating system like Alpine Linux or Ubuntu, or a language-specific runtime environment like Python, Node.js, or Java. It provides the core environment and necessary system libraries.
* **Application Code:** The actual code of your application, including source files, scripts, and any other relevant program files.
* **Dependencies:** This includes all the libraries, frameworks, and packages that your application requires to function correctly. These are typically installed using package managers within the image (e.g., apt-get, pip, npm).
* **Environment Variables:** Variables that define the environment in which your application runs, such as database connection strings, API keys, or other configuration settings.
* **Configuration Files:** Any specific configuration files needed for your application or its dependencies.

### Docker Image Lifecycle

1. **Creation:** Images are created using docker build command, which processes the instructions in a Dockerfile to create the image layers.
2. **Storage:** Images are stored locally on the host machine. They can also be pushed to and pulled from docker registries like Docker Hub, AWS, ECR, or Google Container Registry.
3. **Distribution:** Images can be shared bu pushing them to a Docker registry, allowing others to pull and use the same image.
4. **Execution:** Images are executed by runnin, which are instances of these images.

## Dockerfile

A Dockerfile is a text-based script containing a series of instructions that Docker uses to build a Docker image. It serves as a blueprint, defining the environment and configuration necessary to run an application consistently within a container. 

### Key Components of Dockerfile

**1. FROM (The Base)**
This is the most essential instruction and must be the first one in your file. It sets the base image for your build, which is the foundation your image is built on.

* **What it does:** Specifies the parent image to start from (e.g., an operating system like ubuntu, or a pre-built application stack like node).

* **Example:**
```
FROM node:18-alpine
```

**2. WORKDIR (The "cd" Command)**
This is a best practice. It sets the working directory for any subsequent RUN, CMD, ENTRYPOINT, COPY, and ADD instructions.

* **What it does:** It's like running cd /app in a terminal. If the directory doesn't exist, Docker will create it for you. This keeps your file system organized.

* **Example:**
```
WORKDIR /app
```
**3. COPY (Adding Your Files)**
This instruction is used to get your application code and other files from your local machine (the "build context") into the image.

* **What it does:** Copies files or directories from a source on your host machine to a destination in the image's filesystem.

* **Example:**
```
# Copy a single file
COPY package.json .

# Copy all files from the current directory
COPY . .
```

**4. RUN (Installing Dependencies)**
This instruction executes any command during the build process to create a new layer. This is how you install software, update packages, or create directories.

* **What it does:** Runs commands inside the image, such as apt-get for Debian/Ubuntu-based images or npm install for Node.js.

* **Example:**
```
# Example for a Debian/Ubuntu base
RUN apt-get update && apt-get install -y vim

# Example for a Node.js base
RUN npm install
```

**5. EXPOSE (Documenting Ports)**
This instruction is metadata. It informs Docker that the container listens on the specified network ports at runtime.

* **What it does:** It's a form of documentation between the image builder and the person running the container.

* **Important:** EXPOSE does not actually publish the port. You still must use the -p flag when running docker run (e.g., docker run -p 8080:3000 ...) to map the port.

* **Example:**
```
EXPOSE 3000
```

**6. CMD (The Default Command)**
This instruction provides the default command to be executed when a container starts from your image.

* **What it does:** Sets the program that will run. There can only be one CMD. If you provide a command at the end of docker run ... (e.g., docker run my-image ls), it will override the CMD.

* **Example (exec form, preferred):**
```
CMD ["node", "server.js"]
```

### tying it all together: An Example
Here is a simple Dockerfile for a Node.js application that uses all these components.
```
# 1. Start from a lightweight Node.js base image
FROM node:18-alpine

# 2. Set the working directory inside the container
WORKDIR /app

# 3. Copy package files (for dependencies)
# We copy this first to take advantage of Docker's layer caching.
COPY package.json package-lock.json ./

# 4. Install dependencies
RUN npm install

# 5. Copy the rest of the application code
COPY . .

# 6. Expose the port the app will run on
EXPOSE 3000

# 7. Define the default command to start the app
CMD ["node", "index.js"]
```

## Docker Container
A Docker container is a live, running instance of a Docker image.

If a Docker image is a read-only blueprint (or a recipe) that contains your application, its dependencies, and instructions, then a container is the actual house built from that blueprint (or the cake you baked from that recipe).

## Registry
A registry is a centralized server for storing and distributing your Docker images.

Think of it as the GitHub for Docker images. It's a place where you can push (upload) your images after you build them, and pull (download) them to any other machine.

### Components of a Docker registry

**1. Repository** 

This is the specific collection of related images, usually for one application, with different versions (tags).

**2. Tags**

A tag is simply a human-readable pointer or alias that points to a specific manifest.

  * **Function:** It gives a friendly name to a specific version of an image.
  
  * **How it works:** When you push my-app:1.1, the registry creates a tag 1.1 in the my-app repository and points it at the manifest for your newly pushed image. You can have multiple tags (e.g., 1.1 and latest) pointing to the exact same manifest.

### Types of Registries

**1. Public Registries:**

* These registries are accessible to anyone and typically host a vast collection of pre-built Docker images, including official images and those contributed by the community.

* Docker Hub: is the most prominent example, serving as the default and largest public registry, offering both public and private image storage options.

**2. Private Registries:**

* These registries restrict access to authenticated users and are commonly used by individuals, teams, or organizations to store proprietary, confidential, or sensitive Docker images.
* Private registries offer greater control over image access and security.

**3. Third Party Registries**

* Amazon Elastic Container Registry (ECR)
* Google Artifact Registry (formerly Google Container Registry)
* Azure Container Registry (ACR)
* GitHub Container Registry
* GitLab Container Registry
