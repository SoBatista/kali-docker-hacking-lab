# 🐳 Docker Hygiene Routine
Keeping your Docker environment clean and secure is essential. Over time, old containers and images can take up space, include outdated packages, or expose unpatched vulnerabilities.
This routine helps you stay tidy and as safe as possible, with minimal effort.

> ⚠️ **Warning:** The commands below may permanently remove containers and images. Make sure you've saved any important data before proceeding.

---

## Clean Up Containers & Images
1. First, change into the directory where you manage your Docker images and containers.

```bash
cd /path/to/your/project
```

2. Check for leftover containers:

```bash
docker ps -a
```

3. Remove all the containers from this project:

```bash
docker compose down --remove-orphans
```

4. Check for dangling or unused images:

```bash
docker images
```

5. To remove a specific image:

```bash
docker rmi ID
```

6. To remove all the images from this project:

```bash
docker compose down --rmi all
```

> **Note:** This command only works if the image was built using Docker Compose.

---

## Scan Images with *Trivy*
### Install Trivy:

1. First update the system:

```bash
sudo apt update
```
2. Install Trivy:

```bash
sudo apt install trivy
```

### Before creating a container, scan your image for known vulnerabilities: 

1. List all images:

```bash
docker images
```

2. Then scan your image:

```bash
trivy image <container_name>
```

