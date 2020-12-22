# Containers for scientific computing

This repository is a work-in-progress collection of tools, recipes, and workflows for container-based scientific computing. It might serve as a basis for "best practice" developments, especially when it comes to the actual portability of containers for scientific computing applications.

## Docker

### Host user mapping

Files created inside a Docker container, especially those that are (in good practice!) created by non-root container user accounts, will have unpredictable ownerships when inspected from the host system.
Furthermore, files created with the host system user account will likely be inaccessible from inside a container environment.

#### Docker flags: Build-in approach

You might simply use `docker run --user $(id -u):$(id -g) --group-add <put-container-group-here>`.
This explicitely runs the container process as host system user, and therefore prevents the file permission problem.
The `--group-add` flag assigns the host system user to a group inside the container environment, which might preserve the rights of the default container user for the host user, and with that to e.g. interactively adapt the container environment.
For this approach, no `Dockerfile` adaptions are necessary.
Since the host system user might not exist inside the container environment (i.e. listed in the `/etc/passwd` and `/etc/groups`) some applications might refuse to work.
In that case, the wrapper script approach below can be used.
It can also be used to prevent the shell sessions warnings about non-existing users and/or group entries.

#### Wrapper script: Merging user account information

The wrapper script [as-host-user.sh](as-host-user.sh) automates the above solution, and provides a hacky, but clean and quick way of solving the Docker volume file permission problem described above.
It does not require any `Dockerfile` adaptions (that might impede container portability across several host systems, such as e.g. with specifying the target system host user during a Docker image build) and/or changing any Docker daemon / host system defaults (that could have unconsidered side effects, especially if set up by unexperienced users).

The drawback of wrapping a `docker run` is that, depending on the file ownership settings in the container, that the possibility of interactively adapting the container environment is lost.
You might be able to use `--group-add <put-container-group-here>` (see above!) to circumvent this, though.
