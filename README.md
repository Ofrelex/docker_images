# Working with Docker Images

Got it ✅ — thanks for sharing the feedback.

Below is a compact, practical workflow to complete a project based on the content in your screenshot (Introduction → Pulling images). It covers everything from installing Docker to building, inspecting, pushing, and cleaning images — with commands and deliverables you can submit as evidence.

> *Containerize a static HTML webpage using Nginx on an Ubuntu EC2 instance, expose port 80, and verify it in a browser. Then push the custom image to Docker Hub.*

Let me break down the **correct step-by-step process** that directly matches the assignment:

---

## 🔹 Correct Step-by-Step Project Workflow

### 1. Launch an EC2 Instance

1. Log into your **AWS Console** → go to **EC2 service**.

2. Launch a new instance:

   * Choose **Ubuntu Server 22.04 LTS (x86\_64)**.
   * Select instance type: **t2.micro (free tier)**.
   * Configure key pair for SSH.
   * Configure **Security Group**:

     * Allow **SSH (port 22)** from your IP.
     * Allow **HTTP (port 80)** from anywhere (0.0.0.0/0).
   * Launch instance.

3. SSH into the instance:

   ```bash
   ssh -i your-key.pem ubuntu@<EC2-Public-IP>
   ```

---

### 2. Install Docker on EC2

Run the following on your EC2:

```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

Check:

```bash
docker --version
```

---

### 3. Prepare Static Website Content

1. On your EC2, create a project folder:

   ```bash
   mkdir ~/nginx-web && cd ~/nginx-web
   ```
2. Add a simple `index.html`:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
     <title>My Dockerized Webpage</title>
   </head>
   <body>
     <h1>Hello from Docker + Nginx on AWS EC2!</h1>
   </body>
   </html>
   ```

---

### 4. Write the Dockerfile

Use **official Nginx image** as base:

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

---

### 5. Build and Run the Docker Image

1. Build:

   ```bash
   sudo docker build -t mynginx:1.0 .
   ```
2. Run container and expose **port 80**:

   ```bash
   sudo docker run -d -p 80:80 mynginx:1.0
   ```
3. Verify container is running:

   ```bash
   sudo docker ps
   ```

---

### 6. Verify in Browser

* Open browser and go to:
  `http://<EC2-Public-IP>`
* You should see: **Hello from Docker + Nginx on AWS EC2!**

---

### 7. Push Custom Image to Docker Hub

1. Log in:

   ```bash
   sudo docker login
   ```
2. Tag image:

   ```bash
   sudo docker tag mynginx:1.0 yourdockerhubusername/mynginx:1.0
   ```
3. Push:

   ```bash
   sudo docker push yourdockerhubusername/mynginx:1.0
   ```

---

### 8. Final Deliverables (for submission)

* **Dockerfile** (with Nginx base + COPY index.html).
* **index.html** (static page).
* Screenshot of:

  * EC2 instance running.
  * `docker ps` showing container running.
  * Browser showing webpage via EC2 public IP.
  * Docker Hub repository with your pushed image.
