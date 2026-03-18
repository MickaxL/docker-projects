![Docker](https://img.shields.io/badge/Docker-29.3+-blue?logo=docker)
![Nginx](https://img.shields.io/badge/Nginx-alpine-green?logo=nginx)
![License](https://img.shields.io/badge/License-MIT-lightgrey)
![Status](https://img.shields.io/badge/Status-Done-brightgreen)

#  nginx-static — Docker Nginx Static Site

A simple static HTML page served by Nginx inside a Docker container.  
This is the first project in my `docker-projects` learning repository.

>  Goal: Learn the basics of Docker — Dockerfile, image build, container run, port mapping.

---

##  Project Structure

```
nginx-static/
├── Dockerfile       # Instructions to build the image
└── index.html       # Static page served by Nginx
```

---

##  How It Works

```
Browser (localhost:8080)
        ↓
  Docker container
        ↓
  Nginx (port 80)
        ↓
  index.html
```

The Dockerfile tells Docker to:
1. Start from the official `nginx:alpine` image (lightweight Linux + Nginx)
2. Copy our `index.html` into the folder Nginx serves by default
3. Expose port 80 inside the container

When we run the container, we map **port 8080 on our machine** to **port 80 inside the container**.

---

##  Dockerfile explained

```dockerfile
FROM nginx:alpine
# Base image — Alpine Linux + Nginx pre-installed (~23MB only)

COPY index.html /usr/share/nginx/html/index.html
# Copy our HTML file into Nginx's default serving folder

EXPOSE 80
# Document that the container listens on port 80
```

---

##  Usage

### Build the image
```bash
docker build -t nginx-static .
```
- `docker build` → creates an image from the Dockerfile
- `-t nginx-static` → gives it a name (tag)
- `.` → uses the current folder as context

### Run the container
```bash
docker run -d -p 8080:80 --name mon-nginx nginx-static
```
- `-d` → detached mode (runs in background, doesn't block terminal)
- `-p 8080:80` → maps port 8080 (host) to port 80 (container)
- `--name mon-nginx` → gives the container a readable name
- `nginx-static` → the image to use

### Access the site
Open your browser: **http://localhost:8080**

---

##  Useful commands

```bash
# See running containers
docker ps

# See all containers (including stopped)
docker ps -a

# See logs
docker logs mon-nginx

# Enter the container
docker exec -it mon-nginx sh

# Stop the container
docker stop mon-nginx

# Remove the container
docker rm mon-nginx

# Remove the image
docker rmi nginx-static
```

---

##  What I learned

- How to write a basic `Dockerfile`
- The difference between an **image** (the recipe) and a **container** (what runs)
- How **port mapping** works with `-p host:container`
- How to run a container in **detached mode** with `-d`
- Basic Docker commands: `build`, `run`, `ps`, `stop`, `rm`

---

## 🧪 Tested On

| OS | Docker version |
|----|---------------|
| Ubuntu 24.04 (WSL2) | 29.3.0 |

---

## 👤 Author

**Mickaël Paquet** — Junior Cybersecurity  
[LinkedIn](https://www.linkedin.com/in/mickael-paquet7a0638312) · [GitHub](https://github.com/MickaxL)