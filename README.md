# 🐳 Yonder Docker Workshop

---

**Opdracht 1: Install Docker**  

- [Windows Install](https://docs.docker.com/desktop/setup/install/windows-install/)
- [Mac Install](https://docs.docker.com/desktop/setup/install/mac-install/)
- [Linux Install](https://docs.docker.com/desktop/setup/install/linux/)

---

**Opdracht 2: Docker CLI (Basic)**  

```Bash
# Runs docker container with hostport 1111 and containerport 80 in detached mode.
docker run -d -p 11111:80 docker/getting-started

# Runs docker container with hostport 8080 and containerport 80 in detached mode.
docker run -d -p  8080:80 nginx:latest

# Runs docker container with default container settings in detached mode.
docker run -d mysql:latest
```

**Opdracht 2: Docker CLI (Advanced)**  

```Bash
### Runs docker container with the name wordpress-mysql with the env vars for the root password, the database name and it runs in detached mode.
docker run -d --name wordpress-mysql -e MYSQL_ROOT_PASSWORD=dit-is-super-geheim -e MYSQL_DATABASE=wordpress mysql:latest

### Runs docker container with the name wordpress with the env vars to connect wordpress to the database, with hostport 8080, container port 80, that links wordpress to the database container and it runs in detached mode.
docker run -d --name wordpress -e WORDPRESS_DB_HOST=wordpress-mysql:3306 -e WORDPRESS_DB_NAME=wordpress -e WORDPRESS_DB_USER=root -e WORDPRESS_DB_PASSWORD=dit-is-super-geheim -p 8080:80 --link wordpress-mysql:mysql wordpress
```

Access WordPress at: [http://localhost:8080](http://localhost:8080)

---

**Opdracht 3: Creating the manifest**  

## 📁 Dockerfile for MySQL

```Dockerfile
# Dockerfile.mysql
#FROM gets the base image for the build.
FROM mysql:latest

#ENV sets the required and optional variables for the mysql database.
ENV MYSQL_ROOT_PASSWORD=dit-is-super-geheim
ENV MYSQL_DATABASE=wordpress
```

## 📁 Dockerfile for WordPress

```Dockerfile
# Dockerfile.wordpress
#FROM gets the base image for the build.
FROM wordpress:latest

#ENV sets the required variables for the wordpress application.
ENV WORDPRESS_DB_HOST=wordpress-mysql:3306
ENV WORDPRESS_DB_NAME=wordpress
ENV WORDPRESS_DB_USER=root
ENV WORDPRESS_DB_PASSWORD=mdit-is-super-geheim
```

---

**Opdracht 4: Building the images**  

## 🏗️ Build the Images

```bash
# Build MySQL image with a tag based on the Dockerfile in the ./mysql-wordpress folder. (Change the tag to something like: mysql:yonder)
docker build -t francois013/yonder-workshop:mysql ./mysql-wordpress

# Build WordPress image with a tag based on the Dockerfile in the ./wordpress folder. (Change the tag to something like: wordpress:yonder)
docker build -t francois013/yonder-workshop:wordpress ./wordpress
```

---

**Opdracht 5: Run the containers**  

## 🚀 Run the Containers

```bash
# Start MySQL container
docker run -d --name francois013/yonder-workshop:mysql

# Start WordPress container
docker run -d --name wordpress -p 8080:80 --link wordpress-mysql:mysql francois013/yonder-workshop:wordpress
```

Access WordPress at: [http://localhost:8080](http://localhost:8080)

---

## ⚠️ Notes

- These Dockerfiles **bake secrets into the image** (like DB passwords). Do **not** use this approach for production.
- For a secure and flexible alternative, prefer `docker-compose.yml` with `.env` files.
