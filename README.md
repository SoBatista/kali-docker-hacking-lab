# kali-docker-hacking-lab

[![Watch the video](https://img.youtube.com/vi/0SZyLPw1keM/maxresdefault.jpg)](https://www.youtube.com/watch?v=0SZyLPw1keM)

## Install Docker on Kali Linux

### Update the system:
```bash
sudo apt update
```

### Install the basics to securely add Docker’s official repo:
```bash
sudo apt install -y ca-certificates curl gnupg
```

### Install and verify Docker’s GPG key:
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

### Add Docker’s repository to your system:
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/debian $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Update the system again:
```bash
sudo apt update
```

### Install Docker Engine and Docker Compose:
```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## Building the Secure Hacking Container

### Create a working directory:
```bash
mkdir ~/hacking-sandbox && cd ~/hacking-sandbox
```
> The `hacking-sandbox` directory will hold everything related to your secure hacking environment.

### Create a directory inside for results:
```bash
mkdir results
```
> The `results` directory is where everything you do inside the container will be saved.

---

### Create a `Dockerfile`:
```bash
touch Dockerfile
```

**Paste this inside the Dockerfile:**
```dockerfile
FROM kalilinux/kali-rolling

# Create non-root user
RUN useradd -m sandbox

# Install basic tools
RUN apt update && apt install -y curl wget git iputils-ping net-tools

# Use non-root user
USER sandbox
WORKDIR /home/sandbox

CMD ["bash"]
```

---

### Create the `docker-compose.yml` file:
```bash
touch docker-compose.yml
```

**Paste this inside `docker-compose.yml`:**
```yaml
version: '3.8'

services:
  kali:
    build: .
    container_name: kali_sandbox
    network_mode: none  # No internet access
    #network_mode: bridge  # Uncomment this for internet access
    user: sandbox
    volumes:
      - ./results:/home/sandbox/results
    stdin_open: true
    tty: true
    restart: "no"
```

---

## Using the Container

### Build the container:
```bash
docker compose build
```

### Start the container:
```bash
docker compose run kali
```

### Exit the container:
```bash
exit
```

### Delete all containers:
```bash
docker compose down --remove-orphans
```

### Save output files:
```bash
echo "scan results" > ~/hacking-sandbox/results/scan.txt
```
> Replace `"scan results"` with your actual output. Files will be stored in the `results` folder on your host.

---

### Enable internet access (when needed):

1. Exit the container.
2. Edit `docker-compose.yml` and **uncomment** `network_mode: bridge`, then **comment out** `network_mode: none`.

Like so:
```yaml
#network_mode: none  # No internet access
network_mode: bridge  # With internet access
```

3. Rebuild and run:
```bash
docker compose build
docker compose run kali
```

> **Only enable internet access when absolutely needed.**

---

### View your saved results:
```bash
cat ~/hacking-sandbox/results/<filename>
```

---

## Add an Alias for Fast Access

1. Edit your `.bashrc`:
```bash
nano ~/.bashrc
```

2. Add this to the end:
```bash
alias hackbox='docker compose run kali'
```

3. Apply the changes:
```bash
source ~/.bashrc
```

4. Now you can start your sandbox with:
```bash
hackbox
```

---

## And you're done!
Your secure, offline-ready hacking container is now fully set up and ready to go. Use it safely, and hack ethically.
