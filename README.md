# DockerForROS2Development
Docker images for Easy ROS2 development

## Iron Image
```bash
# Pull
docker pull ghcr.io/soham2560/iron:latest
```

```bash
# Build
docker build -t ghcr.io/soham2560/iron -f iron/Dockerfile iron --build-arg USERNAME="sivan2560"
```

## Create Tag and Push

> Always create tags from the main branch

```bash
git checkout main
git pull
# Replace X.X.X with the version number
git tag vX.X.X
git push origin --tags
```
