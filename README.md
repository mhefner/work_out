# Weekly Workout Routine

A responsive web application displaying a comprehensive 5-day workout routine with modern gym-themed styling. This app provides users with a structured weekly fitness plan including lower body strength, upper body push/pull workouts, functional training, and conditioning.

## Features

- **Responsive Design**: Optimized for desktop, tablet, and mobile devices
- **Modern UI**: Dark theme with gym background imagery and smooth hover effects
- **Structured Workouts**: 5-day routine covering all major muscle groups
- **RPE Scale**: Rate of Perceived Exertion ratings for each exercise
- **Equipment Flexible**: Designed for home or gym use with dumbbells, barbells, and resistance bands

## Workout Structure

- **Day 1 (Monday)**: Lower Body Strength
- **Day 2 (Tuesday)**: Upper Body Push (Chest/Shoulders/Triceps)  
- **Day 3 (Wednesday)**: Lower Body Dynamic + Core
- **Day 4 (Thursday)**: Upper Body Pull (Back/Biceps)
- **Day 5 (Friday)**: Full Body Functional + Conditioning
- **Weekend**: Optional light cardio and recovery

## Deployment

### Kubernetes with Argo CD (Recommended)

This application is designed for deployment into Kubernetes clusters using Argo CD for GitOps-based continuous deployment.

#### Prerequisites
- Kubernetes cluster (1.19+)
- Argo CD installed and configured
- kubectl configured to access your cluster

#### Deployment Steps

1. **Fork or clone this repository**
   ```bash
   git clone <your-repo-url>
   cd weekly-workout-routine
   ```

2. **Configure Argo CD Application**
   
   Create an Argo CD application manifest:
   ```yaml
   apiVersion: argoproj.io/v1alpha1
   kind: Application
   metadata:
     name: workout-routine
     namespace: argocd
   spec:
     project: default
     source:
       repoURL: <your-repo-url>
       targetRevision: HEAD
       path: k8s/
     destination:
       server: https://kubernetes.default.svc
       namespace: workout-app
     syncPolicy:
       automated:
         prune: true
         selfHeal: true
       syncOptions:
       - CreateNamespace=true
   ```

3. **Apply the Argo CD Application**
   ```bash
   kubectl apply -f argocd-application.yaml
   ```

4. **Monitor Deployment**
   ```bash
   # Check Argo CD UI or use CLI
   argocd app get workout-routine
   argocd app sync workout-routine
   ```

#### Kubernetes Manifests Structure
```
k8s/
├── namespace.yaml
├── deployment.yaml
├── service.yaml
├── ingress.yaml (optional)
└── configmap.yaml (if needed)
```

### Alternative Deployment Options

#### Docker Hub Image

The application is available as a pre-built Docker image:

**Docker Hub Repository**: https://hub.docker.com/repository/docker/mhefner1983/work_out/general

#### Running with Docker

```bash
# Pull and run the image
docker pull mhefner1983/work_out:dev
docker run -d -p 8080:80 --name workout-app mhefner1983/work_out:dev
```

Access the application at `http://localhost:8080`

#### Docker Compose

Create a `docker-compose.yml` file:

```yaml
version: '3.8'
services:
  workout-app:
    image: mhefner1983/work_out:dev
    ports:
      - "8080:80"
    restart: unless-stopped
    container_name: workout-routine
```

Run with:
```bash
docker-compose up -d
```

#### Manual Kubernetes Deployment

If not using Argo CD, you can deploy manually:

```bash
# Create namespace
kubectl create namespace workout-app

# Deploy the application
kubectl apply -f k8s/ -n workout-app

# Port forward for local access
kubectl port-forward service/workout-service 8080:80 -n workout-app
```

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `PORT` | Application port | `80` |
| `NODE_ENV` | Environment mode | `production` |

### Customization

To customize the workout routine:

1. Edit the `index.html` file
2. Modify the CSS styles for different themes
3. Update exercise lists and RPE values as needed
4. Rebuild and push to your container registry

## Monitoring and Observability

The application includes:
- Health check endpoints
- Prometheus metrics (if enabled)
- Structured logging
- Graceful shutdown handling

## Development

### Local Development

```bash
# Serve locally for development
python -m http.server 8000
# or
npx serve .
```

### Building Custom Image

```bash
# Build your own image
docker build -t your-registry/workout-app:tag .

# Push to registry
docker push your-registry/workout-app:tag
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Technical Stack

- **Frontend**: HTML5, CSS3, JavaScript
- **Containerization**: Docker
- **Orchestration**: Kubernetes
- **Deployment**: Argo CD (GitOps)
- **Styling**: Custom CSS with responsive design
- **Fonts**: Google Fonts (Inter)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

For issues and questions:
- Create an issue in this repository
- Check the Docker Hub repository for image-specific issues
- Review Argo CD documentation for deployment concerns

## Acknowledgments

- Workout routine designed for progressive strength training
- UI inspired by modern fitness applications
- Containerized for cloud-native deployment patterns