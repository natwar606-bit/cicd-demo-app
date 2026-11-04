# Dockerized CI/CD Pipeline using GitHub Actions

![CI](https://github.com/<username>/cicd-demo-app/actions/workflows/ci.yml/badge.svg)

Ek chhoti Flask app, jo har `git push` par apne aap test hoti hai, Docker image banti hai, Docker Hub par jaati hai, aur Linux VM par deploy ho jaati hai.

## Pipeline flow

    git push -> test (pytest) -> Docker build -> Docker Hub push -> self-hosted runner par deploy -> health check

## Tech stack

- Python (Flask), pytest
- Docker, Docker Hub
- GitHub Actions (hosted runner + self-hosted runner)
- CentOS VM

## Endpoints

- `/` : welcome message
- `/health` : health check (`{"status": "ok"}`)

## Local par chalana

    docker run -d -p 5000:5000 <dockerhub-username>/cicd-demo-app:latest
    curl localhost:5000

## Pipeline ki jobs

1. **test**: dependencies install karke pytest chalata hai
2. **build-and-push**: image banata hai, `latest` aur commit-SHA tag ke saath Docker Hub par push karta hai
3. **deploy**: self-hosted runner par naya image pull karke container badalta hai, phir `/health` check karta hai

## Security

Docker Hub credentials GitHub Secrets mein rakhe hain, code mein nahi. Image sirf `main` par push se jaati hai, pull request par sirf test chalta hai.
