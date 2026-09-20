[README.md](https://github.com/user-attachments/files/32433666/README.md)
# Elastiflix -- Movie Search & Analytics Platform

Elastiflix is a containerized movie catalogue and search application
deployed on an **AWS EC2 instance** using **Docker** and the **Elastic
Stack**.

The project provides a Netflix-style movie browsing interface, movie
search, Elasticsearch-powered data storage and search, and Kibana
dashboards for analysing the movie catalogue.

## Project Overview

The application is deployed on an EC2 server and runs as Docker
containers.

### Architecture

``` text
                         AWS EC2
                  ┌─────────────────────┐
                  │                     │
 User Browser ───►│ Frontend :3000      │
                  │        │            │
                  │        ▼            │
                  │ Backend :17700      │
                  │        │            │
                  │        ▼            │
                  │ Elasticsearch :9200 │
                  │        │            │
                  │        ▼            │
                  │ Kibana :5601        │
                  │                     │
                  └─────────────────────┘
```

### Main Components

  Component       Purpose                                         Port
  --------------- ----------------------------------------- ----------
  Frontend        Movie catalogue UI and search interface       `3000`
  Backend         API/server layer                             `17700`
  Elasticsearch   Movie data storage and search engine          `9200`
  Kibana          Analytics and dashboards                      `5601`
  Logstash        Log/data processing where configured        Internal

## Features

-   🎬 Netflix-style movie catalogue
-   🔎 Movie title and cast search
-   🏷️ Movie genre information
-   📅 Movie release-date analysis
-   🌐 Original-language analysis
-   ⚡ Elasticsearch-powered search
-   📊 Kibana dashboards
-   🐳 Docker containerization
-   ☁️ AWS EC2 deployment
-   🔄 Docker Compose based service management
-   📈 Movie catalogue analytics
-   🔍 Elasticsearch query and filtering capabilities

## Technologies Used

-   **AWS EC2**
-   **Docker**
-   **Docker Compose**
-   **Elasticsearch**
-   **Kibana**
-   **Logstash**
-   **Node.js / Express.js**
-   **React**
-   **Elasticsearch APIs**
-   **Linux / Ubuntu**
-   **Git & GitHub**

## Prerequisites

Before running the project, make sure you have:

-   An AWS account
-   An EC2 instance running Ubuntu/Linux
-   Docker installed
-   Docker Compose installed
-   Git installed
-   EC2 Security Group configured for the required ports

### Required Ports

For a public demo, configure the EC2 Security Group according to your
deployment requirements.

Typical application ports:

``` text
3000   → Elastiflix frontend
17700  → Backend API
5601   → Kibana
9200   → Elasticsearch
```

> For production deployments, avoid exposing Elasticsearch (`9200`) and
> internal backend services directly to the public internet unless there
> is a specific security requirement. Prefer exposing the frontend
> through a reverse proxy/load balancer and restricting internal
> services with Security Groups.

## Deploy on AWS EC2

### 1. Connect to EC2

``` bash
ssh -i your-key.pem ubuntu@YOUR_EC2_PUBLIC_IP
```

### 2. Clone the repository

``` bash
git clone YOUR_GITHUB_REPOSITORY_URL
cd Elastiflix
```

### 3. Set the public host

The application uses the EC2 public IP as the public host.

``` bash
export PUBLIC_HOST=YOUR_EC2_PUBLIC_IP
```

Example:

``` bash
export PUBLIC_HOST=3.110.220.248
```

Verify it:

``` bash
printenv PUBLIC_HOST
```

## Run with Docker

If the project contains the provided startup script:

``` bash
chmod +x start.sh
./start.sh
```

The startup script prepares the movie catalogue and starts the Elastic
Stack and Elastiflix services.

You should see output similar to:

``` text
==> Preparing the movie catalogue
6959 movies ready
==> Starting the Elastic Stack + Elastiflix
```

You can check the running containers with:

``` bash
docker ps
```

To view all containers:

``` bash
docker ps -a
```

## Run with Docker Compose

If you want to start the services directly using Docker Compose:

``` bash
docker compose up -d
```

or, on older Docker Compose installations:

``` bash
docker-compose up -d
```

Check the services:

``` bash
docker compose ps
```

View logs:

``` bash
docker compose logs -f
```

View logs for a specific service:

``` bash
docker compose logs -f frontend
docker compose logs -f backend
docker compose logs -f elasticsearch
docker compose logs -f kibana
```

## Access the Application

After the containers are running, open the following URLs in your
browser.

### Elastiflix Website

``` text
http://YOUR_EC2_PUBLIC_IP:3000
```

Example:

``` text
http://3.110.220.248:3000
```

### Backend

``` text
http://YOUR_EC2_PUBLIC_IP:17700
```

### Elasticsearch

``` text
http://YOUR_EC2_PUBLIC_IP:9200
```

Elasticsearch should return cluster information similar to:

``` json
{
  "name": "elasticsearch",
  "cluster_name": "docker-cluster",
  "version": {
    "number": "9.1.0"
  },
  "tagline": "You Know, for Search"
}
```

### Kibana

``` text
http://YOUR_EC2_PUBLIC_IP:5601
```

Kibana can be used to create and view analytics dashboards.

## Elasticsearch Data

The movie catalogue contains approximately **6,959 movie documents** in
the deployed example.

The Elasticsearch data view used for the dashboard is:

``` text
Elastiflix Movies
```

Example fields include:

``` text
release_date
original_language
genres
cast
budget
popularity
production_companies
homepage
imdb_id
id
backdrop_path
adult
```

## Kibana Dashboard

The project includes a movie catalogue dashboard created using Kibana.

The dashboard can display:

-   Total number of movies
-   Movies by genre
-   Releases per year
-   Original language distribution
-   Other movie catalogue statistics

Example dashboard metrics from the deployment:

``` text
Movies indexed: 6,957
```

The dashboard helps visualize the movie dataset stored in Elasticsearch.

## Search Flow

The application follows this basic flow:

``` text
User
  │
  ▼
Elastiflix Frontend
  │
  ▼
Backend API
  │
  ▼
Elasticsearch
  │
  ▼
Search / Filter Results
  │
  ▼
Frontend
```

When a user searches for a movie, the frontend sends the request to the
backend. The backend communicates with Elasticsearch, retrieves matching
documents, and returns the results to the frontend.

## Analytics Flow

``` text
Movie Dataset
      │
      ▼
Elasticsearch
      │
      ▼
Kibana
      │
      ├── Movies by Genre
      ├── Releases per Year
      ├── Original Language
      └── Catalogue Statistics
```

## Useful Docker Commands

### List containers

``` bash
docker ps
```

### Stop containers

``` bash
docker compose down
```

### Restart containers

``` bash
docker compose restart
```

### Rebuild images

``` bash
docker compose build
```

### Rebuild and start

``` bash
docker compose up -d --build
```

### Follow logs

``` bash
docker compose logs -f
```

### Remove containers

``` bash
docker compose down
```

### Remove containers and volumes

``` bash
docker compose down -v
```

> Use `-v` carefully because volumes may contain Elasticsearch data.

## Troubleshooting

### Website is not opening

Check whether the frontend container is running:

``` bash
docker ps
```

Check frontend logs:

``` bash
docker compose logs frontend
```

Check the EC2 Security Group and confirm port `3000` is allowed when
public access is required.

### Backend is not responding

Check:

``` bash
docker compose logs backend
```

Then test:

``` bash
curl http://localhost:17700
```

### Elasticsearch is not responding

Check:

``` bash
docker compose logs elasticsearch
```

Test:

``` bash
curl http://localhost:9200
```

### Kibana is not opening

Check:

``` bash
docker compose logs kibana
```

Then open:

``` text
http://YOUR_EC2_PUBLIC_IP:5601
```

## Stop the Project

If the project includes `stop.sh`:

``` bash
chmod +x stop.sh
./stop.sh
```

Or use Docker Compose:

``` bash
docker compose down
```

## Project Structure

A typical project structure is:

``` text
Elastiflix/
├── backend/
├── frontend/
├── data-loader/
├── elk/
├── img/
├── docker-compose.yml
├── start.sh
├── stop.sh
├── load-dashboards.sh
├── generate-traffic.sh
├── README.md
└── LICENSE
```

The exact structure can vary depending on the version of the project.

## Deployment Summary

``` text
Developer
   │
   ▼
GitHub Repository
   │
   ▼
AWS EC2
   │
   ▼
Docker / Docker Compose
   │
   ├───────────────┐
   ▼               ▼
Frontend         Backend
:3000            :17700
                    │
                    ▼
              Elasticsearch
                 :9200
                    │
                    ▼
                 Kibana
                 :5601
```

## Credits

This project is based on the **Elastiflix** demo concept and uses movie
data associated with **TMDB**.

This deployment README documents the AWS EC2 + Docker + Elastic Stack
setup used for the project.

TMDB and Elastic are referenced for their respective data/product
technologies and are not implied to endorse this deployment.

## Author

**Kiran Kumbhar**

Third-year B.E. student in **Artificial Intelligence & Data Science**,
SPPU, Pune.

### Technologies demonstrated

``` text
AWS EC2
Docker
Docker Compose
Linux
Elasticsearch
Kibana
Node.js
React
Git/GitHub
```

## License

See the `LICENSE` file in the repository for licensing information.
