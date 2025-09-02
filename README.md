# Working with Docker Images

# Step-by-step process

Below is a compact, practical workflow you can follow to complete a project based on the content in your screenshot (Introduction → Pulling images). It covers everything from installing Docker to building, inspecting, pushing, and cleaning images — with commands and deliverables you can submit as evidence.

---

## 1. Prerequisites

1. Install Docker (Docker Desktop on Windows/Mac; Docker Engine on Linux).
2. Verify installation:

   ```bash
   docker version
   docker info
   ```
3. Create (or have) a Docker Hub account (or other registry) for pushing images:

   ```bash
   docker login
   ```

---

## 2. Quick concept check (what an image is)

* **Docker image** = packaged filesystem + metadata + instructions (created from a `Dockerfile`).
* **Container** = a running instance of an image; containers share the host kernel and are lightweight compared to full VMs.

---

## 3. Explore and pull images from Docker Hub

1. **Search the public registry**:

   ```bash
   docker search ubuntu
   ```

   * Use the official and trusted sources (look for `OFFICIAL` star counts).
2. **Pull a specific image**:

   ```bash
   docker pull ubuntu:22.04
   ```
3. **List images on your local machine**:

   ```bash
   docker images
   ```
4. **Tip:** Always pin tags (e.g., `ubuntu:22.04` vs `ubuntu:latest`) to ensure repeatability.

---

## 4. Run and inspect containers (show isolation & behavior)

1. Start an interactive container:

   ```bash
   docker run --rm -it ubuntu:22.04 bash
   ```
2. From the host, list containers and view details:

   ```bash
   docker ps -a
   docker inspect <container-id-or-name>
   docker logs <container-id-or-name>
   docker exec -it <container-id> bash   # attach to running container
   ```
3. Observe isolation: inside the container run `ps aux`, `hostname`, check files under `/proc/self/cgroup`.

---

## 5. Build your own image (create a Dockerfile)

1. Create a minimal app (example: `app.py`) and a `Dockerfile`:

   ```dockerfile
   FROM python:3.11-slim
   WORKDIR /app
   COPY requirements.txt .
   RUN pip install --no-cache-dir -r requirements.txt
   COPY . .
   EXPOSE 8080
   USER 1000
   CMD ["python", "app.py"]
   ```
2. Add `.dockerignore` to exclude unnecessary files (e.g., `.git`, `node_modules`).
3. Build the image:

   ```bash
   docker build -t myapp:1.0 .
   ```
4. Verify the image:

   ```bash
   docker images myapp
   docker history myapp:1.0
   docker image inspect myapp:1.0
   ```

---

## 6. Run your image with common options

* Detached, port mapping, named container, mounted volume:

  ```bash
  docker run -d --name myapp1 -p 8080:8080 -v "$PWD/config":/app/config myapp:1.0
  ```
* Remove container on exit:

  ```bash
  docker run --rm --name temp -it myapp:1.0 bash
  ```

---

## 7. Tagging and pushing to a registry

1. Tag for Docker Hub (or your registry):

   ```bash
   docker tag myapp:1.0 yourusername/myapp:1.0
   ```
2. Push:

   ```bash
   docker push yourusername/myapp:1.0
   ```
3. Pull back on another machine to verify:

   ```bash
   docker pull yourusername/myapp:1.0
   ```

---

## 8. Export/import images (offline transfer)

* Save (export) to tar:

  ```bash
  docker save -o myapp_1.0.tar yourusername/myapp:1.0
  ```
* Load on another host:

  ```bash
  docker load -i myapp_1.0.tar
  ```

---

## 9. Inspect image contents & history

* See layer history:

  ```bash
  docker history myapp:1.0
  ```
* Inspect config and labels:

  ```bash
  docker image inspect myapp:1.0
  ```

---

## 10. Clean up and housekeeping

* Remove an image:

  ```bash
  docker rmi myapp:1.0
  ```
* Remove dangling images:

  ```bash
  docker image prune
  ```
* Full prune (use with caution):

  ```bash
  docker system prune -a
  ```

---

## 11. Security & best practices (apply to images)

* Use minimal, well-maintained base images (alpine / slim variants where appropriate).
* Pin versions in `FROM` lines.
* Use `.dockerignore` to keep images small.
* Avoid embedding secrets into images (use environment variables, secrets management, or mounted files).
* Run as a non-root user (`USER` in Dockerfile).
* Add `HEALTHCHECK` to the Dockerfile for liveness monitoring.
* Scan images for vulnerabilities (`docker scan <image>` or tools like Trivy).

---

## 12. Deliverables / Evidence to include for your project submission

* Repo folder `working-with-docker-images/` containing:

  * `Dockerfile` + `app` code (sample app).
  * `.dockerignore`.
  * A short `README.md` with the exact commands you ran.
  * A text file `evidence.txt` with outputs of:

    * `docker images`
    * `docker ps -a`
    * `docker inspect <image-or-container>`
    * `docker history myapp:1.0`
  * Optional: `myapp_1.0.tar` (saved image) or a link to the pushed image on Docker Hub.
* A 1-page summary that explains:

  * What an image is, and how it differs from a container and a VM.
  * How you built/pulled and pushed an image.
  * A short comparison of runtime/resource footprint versus a VM (observed `docker stats` output if you ran experiments).

---

## Quick checklist (tick off as you go)

* [ ] Docker installed and `docker version` OK
* [ ] Pulled at least one public image (`docker pull`)
* [ ] Ran a container interactively and inspected it (`docker run`, `docker inspect`)
* [ ] Built a custom image from Dockerfile (`docker build`)
* [ ] Tagged & pushed image to a registry (`docker tag` + `docker push`)
* [ ] Saved/loaded an image (`docker save`/`docker load`)
* [ ] Documented commands & outputs in README / evidence file