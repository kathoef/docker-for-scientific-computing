# Containers for scientific computing

This repository is a work-in-progress collection of tools, recipes, and workflows for container-based scientific computing. It might serve as a basis for "best practice" developments, especially when it comes to the actual portability of scientific computing container images.

## Docker

### Host user mapping

Files created inside a Docker container, especially those that are (in good practice!) created by non-root container user accounts, will have unpredictable ownerships when inspected from the host system.
Furthermore, files created with the host system user account will likely be inaccessible from inside a container environment.

#### Wrapper script: Merging user account information

The wrapper script [as-host-user.sh](as-host-user.sh) is a hacky solution, but provides a very quick and user-friendly way of solving the Docker volume file permission problem when it comes to simply executing a container application.
It does not require any `Dockerfile` adaptions (that might impede container portability across several host systems, such as e.g. with specifying the target system host user during a Docker image build) and/or changing any Docker daemon / host system defaults (that could have unconsidered side effects, especially if set up by unexperienced container application users).

The drawback of wrapping a `docker run` is that the possibility of interactively adapting the container environment is lost, as the default Linux file system permissions are designed to act preventive.
Selectively adjusting file permissions with a `chmod a=u <application-folder>` in the `Dockerfile`, such as e.g. for a conda environment and/or an already specified non-root user directory, could then be a workaround.
However, for such use cases better see the `setuidgid` approach below.

#### GOSU, setuidgid, ...
