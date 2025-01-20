# DockerForROS2Development
Docker images for Easy ROS2 development

## Available Images
- `humble`: base `ubuntu:jammy` image with ros2 humble packages with non-root user
- `humble-garden`: base `humble` image with Gazebo Garden
- `iron`: base `ubuntu:jammy` image with ros2 iron packages with non-root user
- `iron-garden`: base `iron` image with Gazebo Garden

## To Use
```bash
# Pull
docker pull ghcr.io/soham2560/<image_name>:latest
```

```bash
# Build from root of repo
docker build -t ghcr.io/soham2560/image_name -f image_name/Dockerfile image_name
```

```bash
# Push
docker push ghcr.io/soham2560/<image_name>:latest
```

Note: If you're using these images in your work, do consider giving appropriate credit.

This work is also inspired by the work of [rahulkatiyar19955](https://www.rahulkatiyar.com/), head over to his profile if you want to see some more cool robotics work.