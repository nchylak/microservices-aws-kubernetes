
# Coworking Space Analytics

This project deploys a Python-based analytics API that provides business analysts with usage insights from a coworking space service. The API runs in a Kubernetes cluster on AWS EKS and connects to a PostgreSQL database deployed as a Kubernetes service. The build pipeline uses AWS CodeBuild to automatically build Docker images and push them to Amazon ECR. AWS CloudWatch Container Insights is enabled to monitor logs and application health.

## How to deploy changes

When a change is made to the main branch, an AWS CodeBuild project automatically creates a new Docker image and pushes it to Amazon ECR. Each image is tagged with both `latest` and the CodeBuild build number. The running application will not update automatically - you must redeploy it to use the new image as follows.

1. *Option 1*: Restart the Deployment

    Since the latest image is always tagged with `latest`, it is enough to restart the pods to force Kubernetes to use it:

    ```bash
    kubectl rollout restart deployment/coworking
    ```

2. *Option 2*: Apply the specific image version

    Edit `deployment/coworking.yaml` and update the image field:

    ```yaml
        containers:
            - name: coworking
            image: <IMAGE_URI>:<IMAGE_VERSION>
    ```

    Then re-apply the manifest:

    ```bash
    kubectl apply -f deployment/coworking.yaml
    ```

## Verify the Deployment

```bash
curl <EXTERNAL_IP>:5153/api/reports/daily_usage
```

Use `kubectl get svc` to find the external IP of the application.

## Troubleshooting

Check the application logs in AWS CloudWatch (under `containerinsights`) or, alternatively, inspect logs directly from a pod:

```bash
kubectl logs <pod-name>
```
