# 🐳 42-Inception

*Inception* is a system administration project I completed as part of the 42 school curriculum. The goal was to create a secure, scalable container-based infrastructure using **Docker**, all configured from scratch.

## What are containers?

Containers allow applications to run in isolated environments, each with its own dependencies and settings. They’re much lighter than virtual machines because they share the host system’s kernel. That means:

* Less memory and disk usage
* Faster startup time
* More efficient scaling

---

## 🖥️ Virtual Machines vs Containers

| Feature         | Virtual Machine     | Docker Container    |
| --------------- | ------------------- | ------------------- |
| Memory Usage    | Heavy (includes OS) | Lightweight         |
| Boot Time       | Slow                | Fast                |
| Isolation       | High (complete OS)  | High (kernel-level) |
| Scalability     | Harder              | Easier              |
| Storage Sharing | No                  | Yes (with volumes)  |

---

## What is Docker?

Before Docker, running an app on someone else’s machine could easily break due to missing dependencies or environment differences.

Docker solved this by letting me package everything my app needs into one self-contained unit: a **Docker image**. This ensures that the app works **the same** on any system that supports Docker.

> No more "It works on my machine" — it just works.

---

## Docker Image vs Container

* **Image** = the blueprint (like a recipe)
* **Container** = the running instance (like the cake)

---

## Docker Architecture

* **Docker Engine**: The core part of Docker

  * **Docker Daemon**: Background process that manages containers
  * **Docker CLI**: Command-line tool to interact with Docker

My process:

1. write a `Dockerfile` describing how to build an image.
2. build the image with `docker build`.
3. run it using `docker run`, which launched a container from the image.

---

## Docker Compose

Since the project involved multiple containers, I used **Docker Compose** to manage them easily. With just one `docker-compose.yml` file, I could:

* Define all services
* Link them together
* Launch them with a single command

---

## Project Architecture: WordPress + MariaDB + NGINX

I containerized a full WordPress setup with secure HTTPS access.

### 🧱 My Containers

1. **MariaDB** – Database container

   * Stores all WordPress data
   * Uses a Docker volume to persist data across reboots

2. **WordPress + PHP-FPM** – Application container

   * Hosts the CMS logic
   * Talks to MariaDB for data
   * Uses its own volume for assets (media, plugins, etc.)

3. **NGINX** – Reverse proxy container

   * Receives all external traffic
   * Forwards requests to WordPress
   * Secured with **TLS v1.2 or v1.3**

### 📡 How They Communicate

* `WordPress` ↔️ `MariaDB` via port 3306
* `NGINX` ↔️ `WordPress` via port 9000
* Visitors ↔️ `NGINX` via port 443

---

## TLS Explained

TLS (Transport Layer Security) encrypts data over the network, ensuring:

* Server authentication
* Privacy of user data
* Optional client-side verification

---

## Project Structure

```bash
inception/
├── Makefile
├── srcs/
│   ├── docker-compose.yml
│   ├── .env
│   └── requirements/
│       ├── nginx/
│       ├── wordpress/
│       └── mariadb/
│
├── data/
└── secrets/
```

Each service has:

* Its own **Dockerfile**
* Config files
* Optional `tools/` and `.dockerignore`

---

## How to Use

To build and launch everything:

```bash
make
```

Then open:

```
https://<your-login>.42.fr
```

Two WordPress users are automatically created:

* One admin (with a custom name)
* One standard user

All data is stored in volumes, so it's persistent across restarts.

---

## What I Learned

This project taught me:

* How to build a containerized infrastructure from scratch
* How to use Docker and Docker Compose
* How to set up TLS, secure configuration, and persistent storage
* Best practices in isolation, networking, and deployment

---

## Project Constraints

Because 42 likes to complicate things : 

* No use of prebuilt images (except Alpine/Debian base)
* One container = one service
* Only one entry point: NGINX on port 443
* No `latest` tags or hardcoded credentials
* `.env` and Docker secrets required
* No hacky tricks like `tail -f` or infinite loops
