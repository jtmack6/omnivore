# Helm Deployment Process Notes

## Overview
This document outlines the process for deploying Omnivore using a custom Helm chart, transitioning from a successful docker-compose deployment.

## Prerequisites
- Working docker-compose deployment of Omnivore
- Kubernetes cluster with Helm installed
- Docker Hub account (jtmack)
- K3s cluster running at home
- Cloudflare DNS configuration
- Internal PostgreSQL server (optional)
- Fish shell (or Zsh/Bash)

## Chart Structure
The Helm chart is located in `self-hosting/helm/omnivore/` with the following structure:
```
omnivore/
├── Chart.yaml           # Chart metadata and dependencies
├── values.yaml          # Default configuration values
└── templates/           # Kubernetes manifest templates
    ├── deployment.yaml  # API deployment
    ├── service.yaml     # API service
    ├── ingress.yaml     # Ingress configuration
    └── _helpers.tpl     # Common template functions
```

## Building and Pushing Custom Images
1. Set up environment variables:
   ```bash
   cd self-hosting
   cp .env.example .env
   ```
   Edit the `.env` file with your specific values:
   ```bash
   # Base URLs for the application
   BASE_URL=https://omnivore.mack.sh
   SERVER_BASE_URL=https://omnivore.mack.sh
   HIGHLIGHTS_BASE_URL=https://omnivore.mack.sh

   # Docker Hub configuration
   DOCKER_HUB_USER=jtmack
   DOCKER_TAG=latest

   # Application environment
   APP_ENV=prod
   ```

2. Build and push images using the provided script:

   For Fish shell:
   ```fish
   # Create a function to load .env file
   function load_env
     for line in (cat .env)
       if string match -q "#*" $line
         continue
       end
       set -l key_value (string split "=" $line)
       if test (count $key_value) -eq 2
         set -gx $key_value[1] $key_value[2]
       end
     end
   end

   # Load environment variables and run the script
   load_env && ./build-and-push-images.sh
   ```

   For Zsh/Bash:
   ```bash
   source .env && ./build-and-push-images.sh
   ```

## Manual Image Building
If the build-and-push-images.sh script isn't working, you can build and push the images manually:

```bash
# Build web image
docker build -t jtmack/omnivore-web:latest \
  --build-arg="APP_ENV=prod" \
  --build-arg="BASE_URL=https://omnivore.mack.sh" \
  --build-arg="SERVER_BASE_URL=https://omnivore.mack.sh" \
  --build-arg="HIGHLIGHTS_BASE_URL=https://omnivore.mack.sh" \
  -f ../packages/web/Dockerfile ..

# Build content-fetch image
docker build -t jtmack/omnivore-content-fetch:latest \
  -f ../packages/content-fetch/Dockerfile ..

# Build API image
docker build -t jtmack/omnivore-api:latest \
  -f Dockerfile ..

# Build migrate image
docker build -t jtmack/omnivore-migrate:latest \
  -f ../packages/db/Dockerfile ..

# Push images
docker push jtmack/omnivore-web:latest
docker push jtmack/omnivore-content-fetch:latest
docker push jtmack/omnivore-api:latest
docker push jtmack/omnivore-migrate:latest
```

For Fish shell users:
```fish
# Build web image
docker build -t jtmack/omnivore-web:latest \
  --build-arg="APP_ENV=prod" \
  --build-arg="BASE_URL=https://omnivore.mack.sh" \
  --build-arg="SERVER_BASE_URL=https://omnivore.mack.sh" \
  --build-arg="HIGHLIGHTS_BASE_URL=https://omnivore.mack.sh" \
  -f ../packages/web/Dockerfile ..

# Build content-fetch image
docker build -t jtmack/omnivore-content-fetch:latest \
  -f ../packages/content-fetch/Dockerfile ..

# Build API image
docker build -t jtmack/omnivore-api:latest \
  -f Dockerfile ..

# Build migrate image
docker build -t jtmack/omnivore-migrate:latest \
  -f ../packages/db/Dockerfile ..

# Push images
docker push jtmack/omnivore-web:latest
docker push jtmack/omnivore-content-fetch:latest
docker push jtmack/omnivore-api:latest
docker push jtmack/omnivore-migrate:latest
```

Make sure you're in the `self-hosting` directory when running these commands. After pushing the images, you can verify they exist in your Docker Hub account by visiting:
https://hub.docker.com/r/jtmack/omnivore-api/tags

## Required Secrets
Before deployment, ensure you have the following secrets created in your Kubernetes cluster:
1. `omnivore-image-proxy-secret`
   - Contains: `IMAGE_PROXY_SECRET`
2. `omnivore-jwt-secret`
   - Contains: `JWT_SECRET`
3. `omnivore-sso-jwt-secret`
   - Contains: `SSO_JWT_SECRET`
4. `omnivore-pg-password`
   - Contains: `PG_PASSWORD`
5. `postgres-admin-user-and-password`
   - Contains: `PGPASSWORD`, `POSTGRES_USER`
6. `omnivore-content-fetch-verification-token`
   - Contains: `VERIFICATION_TOKEN`

## Chart Configuration
1. Create a custom values file:
   ```bash
   cd self-hosting/helm/omnivore
   cp values.yaml my-values.yaml
   ```

2. Edit `my-values.yaml` with your specific settings:
   ```yaml
   # Image settings
   image:
     repository: jtmack/omnivore-api
     tag: latest
     pullPolicy: Always

   web:
     image:
       repository: jtmack/omnivore-web
       tag: latest
       pullPolicy: Always

   # Ingress settings
   ingress:
     enabled: true
     className: traefik
     annotations:
       kubernetes.io/ingress.class: traefik
       cert-manager.io/cluster-issuer: letsencrypt-prod
     hosts:
       - host: omnivore.mack.sh
         paths:
           - path: /
             pathType: Prefix

   # Database settings
   postgresql:
     enabled: true
     auth:
       database: omnivore
       username: omnivore
       password: "your-secure-password"
     primary:
       persistence:
         size: 10Gi
     image:
       repository: sejaeger/postgres-vector
       tag: latest
   ```

## Deployment Steps
1. Update chart dependencies:
   ```bash
   cd self-hosting/helm/omnivore
   helm dependency update
   ```

2. Deploy the chart:
   ```bash
   helm install omnivore . -f my-values.yaml
   ```

3. Verify the deployment:
   ```bash
   # Check pods
   kubectl get pods -l app.kubernetes.io/name=omnivore

   # Check services
   kubectl get svc -l app.kubernetes.io/name=omnivore

   # Check ingress
   kubectl get ingress -l app.kubernetes.io/name=omnivore
   ```

## Upgrading
To upgrade an existing deployment:
```bash
helm upgrade omnivore . -f my-values.yaml
```

## Uninstalling
To remove the deployment:
```bash
helm uninstall omnivore
```

## Database Setup Options

### Option 1: Using Internal PostgreSQL Server
If your internal PostgreSQL server has the vector extension installed:
1. Update the `my-values.yaml` file with your internal PostgreSQL connection details:
   ```yaml
   postgresql:
     enabled: false
   env:
     - name: PG_HOST
       value: your-internal-postgres-host
     - name: PG_PORT
       value: "5432"
     - name: PG_DB
       value: omnivore
     - name: PG_USER
       value: omnivore
   ```

### Option 2: Using Provided PostgreSQL Setup
The chart includes PostgreSQL with vector extension by default. Configure it in `my-values.yaml`:
```yaml
postgresql:
  enabled: true
  auth:
    database: omnivore
    username: omnivore
    password: your-secure-password
  primary:
    persistence:
      size: 10Gi
  image:
    repository: sejaeger/postgres-vector
    tag: latest
```

## DNS and Ingress Configuration
1. Configure Cloudflare DNS:
   - Create an A record for `omnivore.mack.sh` pointing to your home IP
   - Enable Cloudflare proxy (orange cloud)

2. Configure Ingress in `my-values.yaml`:
   ```yaml
   ingress:
     enabled: true
     className: traefik
     annotations:
       kubernetes.io/ingress.class: traefik
       cert-manager.io/cluster-issuer: letsencrypt-prod
     hosts:
       - host: omnivore.mack.sh
         paths:
           - path: /
             pathType: Prefix
   ```

## Troubleshooting
1. Check pod logs:
   ```bash
   kubectl logs -l app.kubernetes.io/name=omnivore
   ```

2. Verify secrets:
   ```bash
   kubectl get secrets | grep omnivore
   ```

3. Check ingress status:
   ```bash
   kubectl describe ingress omnivore
   ```

4. Verify database connection:
   ```bash
   kubectl exec -it $(kubectl get pod -l app.kubernetes.io/name=omnivore -o jsonpath='{.items[0].metadata.name}') -- psql -h $PG_HOST -U $PG_USER -d $PG_DB
   ```

## Known Limitations
- Health checks not implemented
- Resource limits not configured
- Some environment variables are hard-coded and should not be changed

## Future Improvements
1. Implement health checks
2. Add resource limits
3. Consider implementing monitoring
4. Add backup procedures for the database
5. Set up automatic image updates
6. Configure proper SSL/TLS termination
7. Add support for horizontal pod autoscaling
8. Implement proper database backup and restore procedures 