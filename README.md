# Containers for scientific computing

This repository is a work-in-progress collection of tools, recipes, and workflows for container-based scientific computing. It might serve as a basis for "best practice" developments, especially when it comes to the actual portability of scientific computing container images.

## Docker

### Host user mapping

Files created inside a Docker container, especially those that are (in good practice) created by non-root container user accounts, will have unpredictable ownerships when inspected from the host system.
Furthermore, files created with the host system user account will likely be inaccessible from inside a container environment.

The wrapper script [as-host-user.sh](as-host-user.sh) provides a very quick, user-friendly and portable solution to the Docker volume file permission problem and does not require any Dockerfile adaptions (that might impede container portability across several host systems) and/or changing any Docker daemon or host system defaults (that could have unconsidered side effects, especially for unexperienced users).
A stumbling block is that you might loose the ability to interactively adapt a container's system environment.
