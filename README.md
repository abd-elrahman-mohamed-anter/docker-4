# DevOps Project: Docker & Container Orchestration

This project demonstrates the use of Docker Compose to manage and scale a multi-container application.

## Prerequisites
- Docker  
- Docker Compose  

---

## 1. Initial Deployment & Container Status
The first step involved running the Docker Compose setup. Despite a warning about a missing Dockerfile, the command successfully started the database container (`clinic-db`) and the application container (`spring-bit-clinic`).

```bash
docker compose up -d
```
![1](1.png)

---

## 2. Verifying the Running Containers
To check the status of all containers managed by the Docker Compose file, the following command was used:

```bash
docker compose ps
```

The output confirmed that both the database (`clinic-db`) and the application (`spring-bit-clinic`) were up and running.
![2](2.png)
---

## 3. Accessing the Application
The application was accessed successfully in a web browser. The URL `localhost:32775` indicated that the application was running, showing the "Welcome" page of the Spring PetClinic application.
![3](3.png)
---

# Scaling Spring PetClinic with Docker Compose & Nginx Load Balancer 🐳

## 4. Scaling the Spring Application

To scale the application into multiple replicas, run:

```bash
docker compose -f docker-compose-replica.yml up -d --scale spring-app=5
```

* `docker compose` → Starts Docker Compose (v2).
* `-f docker-compose-replica.yml` → Use the replica-specific compose file.
* `up -d` → Launch containers in detached (background) mode.
* `--scale spring-app=5` → Run **5 instances** of the Spring PetClinic application.

⚠️ Note: Docker may warn that `version:` in the Compose file is obsolete — you can safely remove it.

📸 Example output:
![scale](scale.png)

---

## 5. Verifying Running Containers

Check which containers are running with:

```bash
docker compose ps
```

This lists:

* **db** → 1 MySQL container.
* **spring-app** → 5 replicas (`spring-app-1` … `spring-app-5`).
* **nginx** → A single reverse proxy instance.

📸 Example output:
![ps](ps.png)

---

## 6. Testing Load Balancing with Nginx

Nginx was configured with an **upstream block** and a custom response header `X-Served-By` to identify which replica handled each request.

Run the following test:

```bash
for i in {1..30}; do curl -s -i http://localhost:8888 | grep X-Served-By; done
```

Explanation:

* `for i in {1..30}` → Loop executes 30 requests.
* `curl -s -i http://localhost:8888` → Sends HTTP requests to the reverse proxy.
* `grep X-Served-By` → Filters only the header that shows which replica responded.

📸 Example output:
![curl-loop](for.png)

👉 The responses show different container IPs like:

```
X-Served-By: 172.23.0.3:8080
X-Served-By: 172.23.0.4:8080
X-Served-By: 172.23.0.5:8080
X-Served-By: 172.23.0.6:8080
```

This confirms that Nginx **distributes traffic across multiple replicas**.

---

## 7. Final Result

* ✅ **5 Spring PetClinic replicas** running in parallel.
* ✅ **Nginx reverse proxy** load balances incoming traffic.
* ✅ **X-Served-By header** makes it easy to see which replica processed each request.

