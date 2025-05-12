
# 🐧 Docker & Linux in Windows 11 via WSL2 – Complete Setup Guide

This document summarizes the full progress from enabling Linux in Windows 11 to successfully running and managing Docker containers, based on the tutorial content and your practical execution.

---

## ✅ 1. Enable Linux in Windows 11 via WSL2

### Steps:
```bash
wsl --install
```
- Installs Ubuntu (default).
- After installation, you’ll be prompted to:
  - **Enter UNIX username**
  - **Create and confirm a password** (input hidden)

---

## ✅ 2. Update and Upgrade Ubuntu
```bash
sudo apt update && sudo apt upgrade -y
```

---

## ✅ 3. Install Docker in Ubuntu (WSL2)

### Install dependencies:
```bash
sudo apt install -y ca-certificates curl gnupg lsb-release
```

### Add Docker GPG key:
```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

### Add Docker repository:
```bash
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu   $(lsb_release -cs) stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Update & install Docker engine:
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## ✅ 4. Enable Docker Without Sudo
```bash
sudo usermod -aG docker $USER
```

> **Then restart the terminal** or run:
```bash
newgrp docker
```

---

## ✅ 5. Test Docker Installation
```bash
docker run hello-world
```

Expected output:
> Hello from Docker!  
> Confirms installation works.

---

## ✅ 6. List All Containers
```bash
docker ps -a
```

---

## ✅ 7. Run nginx Server Container
```bash
docker run -d nginx
```

> Starts nginx in background.

### Check Running Containers:
```bash
docker ps
```

---

## ✅ 8. Map nginx to Port 8082
```bash
docker run -d -p 8082:80 nginx
```

### Visit in browser:
```
http://localhost:8082
```

---

## ✅ 9. Stop nginx Container
```bash
docker ps
docker stop <CONTAINER_ID>
```

---

## ✅ 10. Run MySQL Container (Travail à Faire 2)

Delete it and recreate fresh

docker rm my-phpmyadmin

docker run -d --name my-mysql -e MYSQL_ROOT_PASSWORD=secret -p 3306:3306 mysql
```

### Enter MySQL Container:
```bash
docker exec -it my-mysql bash
```

### Access MySQL inside:
```bash
mysql -u root -p
# Password: secret
```

---

# ✅ Summary:
- Ubuntu via WSL2 ✔️
- Docker Engine Installed ✔️
- Hello-World Image ✔️
- nginx Web Server ✔️
- MySQL Database ✔️

You're now ready to build and manage full Dockerized environments in your Windows 11 system.

---

📦 *Prepared with reference to the PDF tutorial by Pr. Hilmani Adil*

---

## 📦 Bonus: docker-compose.yml for MySQL + phpMyAdmin

To manage both MySQL and phpMyAdmin together, create a `docker-compose.yml` file:

```yaml
version: '3.8'

services:
  mysql:
    image: mysql
    container_name: my-mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: secret
    ports:
      - "3306:3306"

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: my-phpmyadmin
    restart: always
    ports:
      - "8081:80"
    environment:
      PMA_HOST: mysql
    depends_on:
      - mysql
```

### 🚀 To run:
```bash
docker compose up -d
```

Then open your browser at: [http://localhost:8081](http://localhost:8081)

---


---

## 🛠️ How to Create and Run `docker-compose.yml`

Follow these steps inside your Ubuntu (WSL2) terminal:

### 📝 1. Create the file using nano
```bash
nano docker-compose.yml
```

### 🧾 2. Paste this content into the file:
```yaml
version: '3.8'

services:
  mysql:
    image: mysql
    container_name: my-mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: secret
    ports:
      - "3306:3306"

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: my-phpmyadmin
    restart: always
    ports:
      - "8081:80"
    environment:
      PMA_HOST: mysql
    depends_on:
      - mysql
```

### 💾 3. Save and exit nano
- Press `Ctrl + O` to save the file
- Press `Enter` to confirm the name
- Press `Ctrl + X` to exit the editor

---

### 🐳 4. Run the containers
```bash
docker compose up -d
```

This will pull the Docker images and start both MySQL and phpMyAdmin services.

---

### 🌐 5. Access the phpMyAdmin dashboard
Open your browser and visit:
```
http://localhost:8081
```

Login credentials:
- **Username**: `root`
- **Password**: `secret`
- **Server**: `mysql`

---
