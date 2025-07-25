# 🐳 Kali Docker Hacking Lab

A clean, container-based hacking lab built using **Docker inside a Kali Linux virtual machine (VM)**.

This project helps you:
- Run a sandboxed, non-root Kali container
- Save your hacking output persistently
- Keep your Docker environment clean and secure
- Scan your Docker images for known vulnerabilities


> ⚠️ **Note:** Docker runs inside Kali, which itself runs as a VM. While this adds some isolation, it's not foolproof. Be cautious when running untrusted tools or code.

---

## 📁 Project Structure

### [`kali-docker-sandbox`](./kali-docker-sandbox)

This folder contains everything you need to build and run a secure hacking container.

**Features:**

- Based on the official `kalilinux/kali-rolling` image
- Runs as a non-root user (`sandbox`)
- No internet access by default (can be enabled by editing `docker-compose.yml`)
- Includes a `results/` directory mounted into the container at:`/home/sandbox/results`

#### 📦 How the `results/` folder works:
- This directory is mounted from the host (your Kali VM) into the container.
- Inside the container, the sandbox user can save any files to `/home/sandbox/results`.
- Those files will remain accessible on the host VM even after the container stops.

**Example: Save Nmap output:**

```bash
nmap -sV -sT -p- <IP> -oN /home/sandbox/results/scan.txt
```

On the host (your Kali VM), you’ll find the saved files under the `results/` directory located next to your Docker files.

---

### [`docker-hygiene`](./docker-hygiene)

This folder contains a Markdown guide for keeping your Docker environment clean and secure.

It includes:
- Commands to list and remove unused containers and images
- How to clean up orphaned or dangling images
- How to scan Docker images for known vulnerabilities using Trivy

Use this to avoid clutter and maintain a clean, secure hacking environment.

---

## ✅ Who Is This For?

- Ethical hackers and learners using Kali inside a VM
- People who want a safe, rebuildable hacking container
- Anyone who wants to avoid cluttering their VM or host system with tools

---

## 📜 License

MIT — see [LICENSE](./LICENSE)

---

Hack clean. Hack smart. Stay ethical. 🧠
