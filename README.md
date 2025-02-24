# Full-Stack Web Application Deployment on AWS

## Overview
This project sets up and deploys a full-stack web application using AWS services, Docker, and GitHub Actions. The backend services are containerized and pushed to Amazon ECR, while the frontend is hosted on S3 and distributed via CloudFront. A CI/CD pipeline automates deployment to EC2 instances, ensuring efficient updates and scalability.

## Architecture
The deployment consists of the following key components:
- **Frontend**: Hosted on AWS S3 and distributed via CloudFront.
- **Backend RDS**: A containerized service using Django and PostgreSQL.
- **Backend Redis**: A caching layer using Redis.
- **CI/CD Pipeline**: Managed through GitHub Actions.
- **Networking**: The services communicate via a Docker network.

## Repository Structure
```
.github/workflows
  ├── frontend-ci-cd.yml
  ├── rds-ci-cd.yml
  ├── redis-ci-cd.yml
backend_rds
  ├── settings.py
  ├── urls.py
  ├── wsgi.py
  ├── core
      ├── urls.py
      ├── views.py
  ├── Dockerfile
  ├── manage.py
  ├── requirements.txt
backend_redis
  ├── core
  ├── Dockerfile
  ├── manage.py
  ├── requirements.txt
frontend
  ├── templates
      ├── index.html
  ├── Dockerfile
  ├── config.json
  ├── package.json
.env
README.md
docker-compose.yml
```

## Deployment
### Prerequisites
- AWS Account
- Docker installed
- AWS CLI configured
- GitHub repository with CI/CD workflows

### Steps to Deploy
#### 1. Clone the repository
```sh
git clone https://github.com/your-repo/fullstack-aws-deployment.git
cd fullstack-aws-deployment
```

#### 2. Set up environment variables
Create a `.env` file and configure:
```
DATABASE_HOST=<your-database-host>
DATABASE_PORT=5432
DATABASE_USER=<your-db-user>
DATABASE_PASSWORD=<your-db-password>
DATABASE_NAME=<your-db-name>
REDIS_HOST=<your-redis-host>
REDIS_PORT=6379
CORS_ALLOWED_ORIGINS=*
```

#### 3. Build and run Docker containers
```sh
docker-compose up --build -d
```

#### 4. Push images to Amazon ECR
```sh
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com

docker tag backend_rds:latest <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/backend_rds:latest
docker push <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/backend_rds:latest

docker tag backend_redis:latest <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/backend_redis:latest
docker push <aws_account_id>.dkr.ecr.us-east-1.amazonaws.com/backend_redis:latest
```

#### 5. Deploy frontend to S3 and CloudFront
```sh
aws s3 sync frontend/templates/ s3://your-bucket-name --delete
aws cloudfront create-invalidation --distribution-id <your-distribution-id> --paths "/*"
```

#### 6. CI/CD with GitHub Actions
Push changes to GitHub, and the CI/CD pipeline will handle deployments automatically.

## Networking
The services communicate using a Docker bridge network:
```yaml
networks:
  app-network:
    driver: bridge
```

## Contributing
Feel free to open issues or submit PRs to improve the setup!

## License
This project is licensed under the MIT License.

