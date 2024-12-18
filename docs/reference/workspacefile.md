---
title: Ocre Workspace File
layout: default
parent: Reference
---

# Ocre Workspace File 

This page will guide you through setting up your development environment for the Ocre runtime using our preconfigured VS Code workspace file. The workspace file provides everything you need to build, flash, and debug the runtime with minimal configuration.

---

## What is a VS Code Workspace?

A Visual Studio Code workspace allows you to manage multiple folders, settings, and tasks as a single, cohesive environment. Workspaces can save you time by automatically loading recommended settings, extensions, and tasks suited for your project.

{: .note}
If you're new to workspaces, we recommend reviewing the [VS Code Workspaces documentation](https://code.visualstudio.com/docs/editor/workspaces) to learn more about their features and usage.

---

## What’s in the "Ocre Workspace" File?

The Ocre runtime workspace file, [**`ocre.code-workspace`**](https://github.com/project-ocre/ocre-runtime/blob/main/ocre.code-workspace), located in the root of the `ocre-runtime` repo, is specifically tailored to help you set up and work with the Ocre runtime efficiently. This file includes:

1. **West Tasks:**
The workspace file includes a predefined West tasks, which simplifies the process of building and flashing the Ocre runtime. These tasks support several board options, which can be selected directly within VS Code.

2. **Recommended Extensions:**
The workspace file also includes extensions that provide additional support for tasks such as code completion, serial communication, and debugging. In general it's recommended to install the plugins found in the workspace file when prompted. 

3. **Debugging Profiles:**
The workspace file also includes debugging profiles specifically for Ocre-compatible boards. These profiles are configured to simplify launching and debugging sessions, so you can quickly load and test the runtime on your target hardware.

---

## How to Use the "Ocre Workspace" File

This section will guide you through running the essential pre-defined tasks in the Ocre workspace file, including build, flash, and debug operations. These tasks make it easy to quickly set up your development environment for various Ocre-compatible boards without needing to configure each setting manually.

### Loading the Workspace File

1. Navigate to the `application` folder in your `ocre-runtime` project directory.
2. Locate `ocre.code-workspace`
3. Double-click the file in your file explorer, then 
4. Click the blue "Open Workspace" button that appears in the bottom right of your editor.

Once loaded, you'll have access to all preconfigured tasks, debug profiles, and recommended extensions.

### Running Build and Flash Tasks

The workspace file includes tasks for both building and flashing the Ocre runtime. Follow these instructions to access and run these tasks:

1. Press `Ctrl + Shift + P` (Windows/Linux) or `Command + Shift + P` (Mac) to open the **Command Palette**.
2. Type `Tasks: Run Task`, select it from the list, and press `Enter`.
3. Choose either **West Build** or **West Flash Application** from the task list.

When prompted, select the board you want to build or flash for, and the workspace file will handle the rest.

### Debugging with Predefined Profiles

The workspace also includes debugging profiles configured for Ocre-compatible boards. To start a debug session:

1. Open the **Run and Debug** view in VS Code by clicking the play icon on the sidebar or pressing `Ctrl + Shift + D` (Windows/Linux) or `Shift + Command + D` (Mac).
2. In the dropdown menu, select the debug profile that matches your target board.
3. Click "Start Debugging" or press `F5` to begin a session. The debugger will launch with the appropriate settings for your board, streamlining the process for testing and troubleshooting.

With these tasks, you can easily build, flash, and debug the Ocre runtime in a consistent and streamlined manner across different Ocre-compatible devices.