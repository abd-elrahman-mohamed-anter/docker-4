# Spring PetClinic Docker Setup

This repository contains Docker Compose configurations to run the **Spring PetClinic** application with MySQL and NGINX.

## Files

- `docker-compose.yml` - Base Docker Compose setup for the app and database.
- `docker-compose-replica.yml` - Replica setup to scale the Spring Boot app.
- `nginx.conf` - NGINX configuration to proxy requests to the Spring Boot application.

## Requirements

- Docker
- Docker Compose

## Run
```bash
docker compose up -d

## with replica 
docker compose -f docker-compose-replica.yml up -d --scale spring-app=4

## then 
Access the app at: http://localhost:8080
