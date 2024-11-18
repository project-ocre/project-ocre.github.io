---
title: Setting Up Your Development Environment 
layout: default
parent: Your First Container 
nav_order: 0
---

# Setting Up Your Development Environment

The Ocre toolchain for C/C++ is provided as a Docker container image to ensure a consistent development environment for building containerzied applications with Ocre. You can utilize this container in two ways:

1. [Using Visual Studio Code with Dev Containers](#developing-with-visual-studio-code): Provides an integrated development experience with full IDE support and container management.

2. [Using Docker Interactively](#interactive-development): Offers a straightforward command-line interface for building and testing Ocre containers.

Choose the method that best suits your development workflow. Both approaches provide the same toolchain capabilities, just with different interfaces and features.

---
## Developing with Visual Studio Code

A Development Container (or dev container) is a Docker container that provides a complete development environment. With a dev container, the VS Code editor operates on your local machine while the Ocre toolchain runs inside the container, ensuring a consistent and isolated development environment.

### Prerequisites

Before you begin, ensure that you have the following tools installed on your development machine:

1. [Docker Desktop](https://www.docker.com/products/docker-desktop/): Required to run Docker containers.
2. [Visual Studio Code](https://code.visualstudio.com/): A lightweight but powerful source code editor.
3. [Dev Containers Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers): This extension allows you to use Docker containers as development environments within VS Code.

{: .note}
The dev container can also be run directly in GitHub via [CodeSpaces](https://docs.github.com/en/codespaces), though you will only be able to build Ocre containers, not load them on your device as the dev container will not have access to your local devices.

### Steps

1. **Install Prerequisites**: Follow the links above to install Docker Desktop, VS Code, and the Dev Containers extension.
2. **Clone the Repository**:
   ```bash
   git clone https://github.com/project-ocre/getting-started.git
   ```
3. **Open Visual Studio Code**: Launch VSCode on your machine.
4. **Open Folder in Container**:
- Run the command `Dev Containers: Open Folder in Container...` from the Command Palette (Cmd + Shift + P or Ctrl + Shift + P).
- Select the folder containing the cloned repository.

Once you select the folder, VS Code will reload. If the container does not exist yet, it will automatically create one and clone the sample repository into an isolated container volume. This process may take a few moments, and progress will be displayed in the lower right corner of the VS Code window. After the container is ready, you can open a terminal within VS Code to access the container environment.

{: .note}
Additional documentation and a step-by-step guide for working with dev containers can be found [here](https://code.visualstudio.com/docs/remote/containers-tutorial).

---

## Interactive Development
The toolchain container can also be launched and used interactively as follows:

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/project-ocre/getting-started.git
   ```

2. **Run the following command** 
```sh
cd getting-started
docker run --rm -it -v $(pwd):/home/ocre ghcr.io/atym-io/atym-c-toolchain:latest
```

{: .note}
At the moment, there is not a docker image that contains the Ocre toolchain. While we're working on getting that up, please use the `atym` toolchain in the above example.

---

## Troubleshooting
This section covers common issues you might encounter when setting up your development environment, along with their solutions.

- If you encounter errors opening the dev container, you likely need to authenticate with the GitHub Container Registry. Follow the authentication steps in the [GitHub documentation](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-with-a-personal-access-token-classic).

---

## Next Steps

Once you're set up your development environment, head over to the [Building And Deploying Your First Container](../first-container) page to learn how to build and deploy your first container using Ocre.