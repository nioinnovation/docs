# Run nio with Docker

Running the nio platform as a [Docker](https://docker.com) container offers several benefits such as better deployment and lifecycle management. However, because different licenses often have different nio binaries, niolabs does not currently make public a downloadable Docker image. Instead, you will have to build and manage your own Docker image based off your nio binary.

## Creating a Docker Image

Once you have downloaded your binary from the [binary downloads page](https://app.n.io/binaries/download), you can build a corresponding Docker image from that binary with the help of the [nio-docker repository](https://github.com/niolabs/nio-docker).

1. Clone the repository and copy your wheel file into the directory
```
git clone https://github.com/niolabs/nio-docker.git nio-docker
cp your-wheel-file.whl nio-docker
```

2. Build the Docker image by passing the name of the wheel file as the `WHEEL_FILE` build argument.
```
docker build -t my-binary-image:latest --build-arg WHEEL_FILE=nio_lite-20171127-py3-none-any.whl .
```

## Running nio as a Docker container

Assuming you have built a Docker image from your binary, you can run the image for different nio project configurations.

To run the nio binary with an empty project just run the image like so:

```
docker run -p 8181:8181 my-binary-image:latest
```

You can also volume mount in an existing project directory on disk like so:

```
docker run -p 8181:8181 -v /path/to/your/project:/nio/project my-binary-image:latest
```