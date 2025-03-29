# Docker Images for ROS 2 Development

This repository provides Docker images designed to simplify ROS 2 development.  These images come pre-configured with the necessary ROS 2 packages and tools, allowing you to get started with your robotics projects quickly and efficiently.

## Available Images

The following Docker images are available:

*   `humble`:  Based on `ubuntu:jammy`, this image includes ROS 2 Humble Hawksbill packages and a non-root user for enhanced security.
*   `humble-garden`:  Extends the `humble` image and includes Gazebo Garden, a powerful 3D robotics simulator.
*   `iron`: Based on `ubuntu:jammy`, this image includes ROS 2 Iron Irwini packages and a non-root user.
*   `iron-garden`: Extends the `iron` image and includes Gazebo Garden.

**Important Note:** The `iron` and `iron-garden` images are currently **under development** and have not been heavily tested.  They may be unstable or contain significant bugs.

## Usage

### 1. Pulling an Image

To download a pre-built image from GitHub Container Registry (ghcr.io), use the following command:

```bash
docker pull ghcr.io/soham2560/<image_name>:latest
```

Replace `<image_name>` with the desired image name (e.g., `humble`, `humble-garden`, `iron`, `iron-garden`). For example:

```bash
docker pull ghcr.io/soham2560/humble-garden:latest
```

### 2. Building an Image (Optional)

If you prefer to build the image locally from the Dockerfile, clone this repository and navigate to the root directory.  Then, use the following command:

```bash
docker build -t ghcr.io/soham2560/<image_name> -f <image_name>/Dockerfile <image_name>
```

Again, replace `<image_name>` with the appropriate image name. For example, to build the `humble-garden` image:

```bash
docker build -t ghcr.io/soham2560/humble-garden -f humble-garden/Dockerfile humble-garden
```

**Explanation of the `docker build` command:**

*   `docker build`: The command to build a Docker image.
*   `-t ghcr.io/soham2560/<image_name>`:  Tags the image with the specified name and registry path.
*   `-f <image_name>/Dockerfile`: Specifies the path to the Dockerfile to use for building the image.
*   `<image_name>`:  The build context, which is the directory containing the Dockerfile and any other files needed for the build.

### 3. Pushing an Image (For Contributors)

If you have made changes to the Dockerfile and want to contribute by pushing the updated image to the registry, use the following command:

```bash
docker push ghcr.io/soham2560/<image_name>:latest
```

**Note:**  You need to be authenticated with `ghcr.io` and have the appropriate permissions to push images to the `soham2560` repository.

## Contributing

Contributions to this project are welcome!  If you have improvements or suggestions, please submit a pull request.

## Acknowledgements

This work is inspired by the work of [rahulkatiyar19955](https://www.rahulkatiyar.com/).  Check out his profile for more interesting  and cool robotics projects.

**Please consider giving appropriate credit when using these images in your work.**