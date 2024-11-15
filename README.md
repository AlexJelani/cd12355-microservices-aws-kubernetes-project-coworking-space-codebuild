# Coworking Space Service Extension
The Coworking Space Service is a set of APIs that enables users to request one-time tokens and administrators to authorize access to a coworking space. This service follows a microservice pattern and the APIs are split into distinct services that can be deployed and managed independently of one another.

For this project, you are a DevOps engineer who will be collaborating with a team that is building an API for business analysts. The API provides business analysts basic analytics data on user activity in the service. The application they provide you functions as expected locally and you are expected to help build a pipeline to deploy it in Kubernetes.

## Architecture Overview

The Coworking Space Service Extension consists of the following components:

1. PostgreSQL Database: Deployed using a Helm Chart for data persistence.
2. Analytics Application: A Python-based API that provides usage reports.
3. AWS CodeBuild: Handles the CI/CD pipeline for building and pushing Docker images.
4. AWS ECR: Stores the Docker images for the application.
5. AWS EKS: Kubernetes cluster for running the application.
6. AWS CloudWatch: Monitors application logs and activity.

## Deployment Process

1. Database Setup:
    - Use Helm to deploy PostgreSQL in the Kubernetes cluster.
    - Initialize the database with seed files.

2. Application Containerization:
    - Build a Docker image for the Python application using the provided Dockerfile.
    - Push the image to AWS ECR using CodeBuild.

3. Kubernetes Deployment:
    - Apply Kubernetes configuration files to create services and deployments.
    - Ensure proper resource allocation and scaling configurations.

4. Monitoring and Logging:
    - Utilize AWS CloudWatch for centralized logging and monitoring.

## Maintenance and Updates

To deploy changes to the application:

1. Update the application code and commit to the GitHub repository.
2. The CodeBuild pipeline will automatically build and push a new Docker image to ECR.
3. Update the Kubernetes deployment to use the new image tag.
4. Apply the updated Kubernetes configuration to roll out the changes.

For database schema changes, update the seed files and apply them to the running PostgreSQL instance.

## Best Practices

- Use semantic versioning for Docker images (e.g., 1.2.1).
- Implement appropriate resource limits and requests in Kubernetes configurations.
- Regularly review and optimize AWS resource usage for cost-effectiveness.
- Implement proper secret management for sensitive information like database credentials.

## Troubleshooting

- Check AWS CloudWatch logs for application issues.
- Use kubectl commands to inspect pod status and logs within the Kubernetes cluster.
- Verify database connectivity using port-forwarding if needed.

For detailed setup instructions and code samples, refer to the sections below.

## Getting Started

### Dependencies
[...]

### Configuration Updates

#### Update ConfigMap
```bash:config/configmap.yaml
kubectl edit configmap coworking-config
```


### Restart Deployment
```bash:restart-deployment.sh
kubectl rollout restart deployment coworking
```
### Database Changes
Connect to Database
```bash:connect.sh
kubectl port-forward svc/postgresql 5432:5432
```

### Apply Migrations
Run the provided SQL files in the db/ directory to apply schema changes and migrations.

### Monitoring and Troubleshooting
* Logs: Available in CloudWatch under /aws/containerinsights/cluster-name/application
*Metrics: Monitor pod health via Kubernetes dashboard
*Database: Access PostgreSQL logs through CloudWatch
## Best Practices
*Use semantic versioning for all image tags
*Store all configuration in version control
*Implement health checks and readiness probes
*Follow the principle of least privilege for service accounts
*Regularly update base images for security
*Implement secrets management for sensitive data