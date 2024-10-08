[![Test Docker Image Status](https://github.com/illumidesk/illumidesk-stacks/workflows/Test%20and%20Push/badge.svg)](https://github.com/illumidesk/illumidesk-stacks/actions?query=branch%3Amain+workflow%3A%22Test+and+Push%22)


# IllumiDesk Docker Stacks

This repo is used to manage IllumiDesk's standard docker images for the IllumiDesk learning environment to power Jupyter Server backends.

## Pre Requisits

- [Docker](https://docs.docker.com/get-docker/)

## Quickstart

1. Install dependencies

```bash
make venv
```

2. Build images

```bash
make build-all
```

3. Run:

Running the image standalone is helpful for testing:

```bash
docker run -p 8888:8888 illumidesk/python-notebook:latest
```

Then, navigate to `http://127.0.0.1:8888` to access your Jupyter Notebook server.

> Refer to [docker's documentation](https://docs.docker.com/engine/reference/run/) for additional `docker run ...` options.

## Customize the Image

1. Add additional Python packages by editing the `./python-notebook/requirements.txt` file.

2. Rebuild end-user and grader images with `make build-all`.

3. Push images to AWS ECR

- [You must first authenticate](https://docs.aws.amazon.com/AmazonECR/latest/userguide/registry_auth.html) to push to AWS ECR repos using `docker` coomands.
- Run `docker push <repo>` where `<repo>` represents the full path to your AWS ECR repository.

1. (Optional) Push images to DockerHub

This step requires creating an Organization account in DockerHub or other docker image compatible registry. The `docker push ...` command will push the image to the DockerHub registry by default. Please refer to the official Docker documentation if you would like to push another registry.

For example, assuming the DockerHub organization is `illumidesk`, the source files are in the `python-notebook` folder, and the tag is `latest`, then the full namespace for the image would be `illumidesk/python-notebook:latest`. Assuming the image has been built, push the image to DockerHub or any other docker registry with the `docker push <image-namespace>:<image-tag>` command:

```bash
docker login
docker push illumidesk/python-notebook:latest
```

## Development and Testing

1. Create your virtual environment and install dev-requirements:

```bash
make venv
```

2. Check Dockerfiles with linter:

```base
make lint-all
```

## References

These images are based on the `jupyter/docker-stacks` images. [Refer to their documentation](https://jupyter-docker-stacks.readthedocs.io/en/latest/) for the full set of configuration and testing options.

## Attributions

- [JupyterHub repo2docker](https://repo2docker.readthedocs.io/en/latest/)
- [jupyter/docker-stacks images](https://github.com/jupyter/docker-stacks)

## License

MIT
