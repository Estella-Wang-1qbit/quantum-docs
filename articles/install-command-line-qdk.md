---
author: bradben
description: Learn how to develop Q# applications in an editor/IDE and run applications from the .NET console
ms.author: v-benbra
ms.date: 02/01/2021
ms.service: azure-quantum
ms.subservice: qdk
ms.topic: quickstart
no-loc: ['Q#', '$$v']
title: Develop with Q# applications in an IDE
uid: microsoft.quantum.install-qdk.overview.standalone
---

# Develop with Q# applications in an IDE

Learn how to develop Q# applications in Visual Studio Code (VS Code), Visual Studio, or with any editor/IDE and run applications from the .NET console. Q# programs can run on their own, without a driver in a host language like C#, F#, or Python.

## Prerequisites for all environments

- [.NET Core SDK 3.1](https://www.microsoft.com/net/download)

## Installation

While you can build Q# applications in any IDE, we recommend using Visual Studio Code (VS Code) or Visual Studio IDE for developing your Q# applications locally. Developing in these environments leverages the rich functionality of the Quantum Development Kit (QDK) extension, which includes warnings, syntax highlighting, project templates, and more.

> [!IMPORTANT]
> If you are working on Linux, you may encounter a missing dependency depending on your particular distribution and installation method (e.g. certain Docker images). Please make sure that the `libgomp` library is installed on your system, as the GNU OpenMP support library is required by the quantum simulator of the QDK. On Ubuntu, you can do so by running `sudo apt install libgomp1`, or `yum install libgomp` on CentOS. For other distributions, please refer to your particular package manager.

Configure the QDK for your preferred environment from one of the following options:

### [VS Code](#tab/tabid-vscode)

1. Download and install [VS Code](https://code.visualstudio.com/download) 1.52.0 or greater (Windows, Linux and Mac). You can install VS Code through a graphical software installer or through the command line in your Linux environment.
2. Install the [QDK for VS Code](https://marketplace.visualstudio.com/items?itemName=quantum.quantum-devkit-vscode).

### [Visual Studio (Windows only)](#tab/tabid-vs)

1. Download and install [Visual Studio](https://visualstudio.microsoft.com/downloads/) 16.3 or greater, with the .NET Core cross-platform development workload enabled.
2. Download and install the [QDK](https://marketplace.visualstudio.com/items?itemName=quantum.DevKit).

> [!NOTE]
> Although there is Visual Studio for Mac, the QDK extension is only compatible with Visual Studio for Windows.

### [Other editors with the command prompt](#tab/tabid-cmdline)

1. Enter the following at the command prompt

```dotnetcli
dotnet new -i Microsoft.Quantum.ProjectTemplates
```

***

## Develop with Q\#

Follow the instructions on the tab corresponding to your development environment.

### [VS Code](#tab/tabid-vscode)

If you are receiving an error "'npm' is not recognized as an internal or external command", in the below steps, install [node.js including npm](https://nodejs.org/en/?azure-portal=true). Alternatively, use our the command line templates to create a Q# project , or use Visual Studio.

To create a new project:

1. Click **View** -> **Command Palette** and select **Q#: Create New Project**.
2. Click **Standalone console application**.
3. Navigate to the location to save the project. Enter the project name and click **Create Project**.
4. When the project is successfully created, click **Open new project...** in the lower right.

Inspect the project. You should see a source file named `Program.qs`, which is a Q# program that defines a simple operation to print a message to the console.

To run the application:

1. Click **Terminal** -> **New Terminal**.
2. At the terminal prompt, enter `dotnet run`.
3. You should see the following text in the output window `Hello quantum world!`

> [!NOTE]
> Workspaces with multiple root folders are not currently supported by the VS Code Q# extension. If you have multiple projects within one VS Code workspace, all projects need to be contained within the same root folder.

### [Visual Studio (Windows only)](#tab/tabid-vs)

Verify your Visual Studio installation by creating a Q# `Hello World` application.

To create a new Q# application:

1. Open Visual Studio and click **File** -> **New** -> **Project**.
2. Type `Q#` in the search box, select **Q# Application** and click **Next**.
3. Enter a name and location for your application and click **Create**.

Inspect the project. You should see a source file named `Program.qs`, which is a Q# program that defines a simple operation to print a message to the console.

To run the application:

1. Select **Debug** -> **Start Without Debugging**.
2. You should see the text `Hello quantum world!` printed to a console window.

> [!NOTE]
> If you have multiple projects within one Visual Studio solution, all projects contained in the solution need to be in the same folder as the solution, or in one of its sub-folders.  

### [Other editors with the command prompt](#tab/tabid-cmdline)

Verify your installation by creating a Q# `Hello World` application.

1. Create a new application:

    ```dotnetcli
    dotnet new console -lang Q# -o runSayHello
    ```

1. Navigate to the application directory:

    ```dotnetcli
    cd runSayHello
    ```

    This directory should now contain a file `Program.qs`, which is a Q# program that defines a simple operation to print a message to the console. You can modfiy this template with a text editor and overwrite it with your own quantum applications.

1. Run the program:

    ```dotnetcli
    dotnet run
    ```

1. You should see the following text printed: `Hello quantum world!`

***

## Next steps

Now that you have installed the Quantum Development Kit in your preferred environment, you can write and run [your first quantum program](xref:microsoft.quantum.tutorial-qdk.random-number).
