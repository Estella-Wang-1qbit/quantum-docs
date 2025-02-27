---
author: mblouin
ms.author: mblouin
ms.date: 06/24/2021
ms.service: azure-quantum
ms.subservice: computing
ms.topic: include
---

## Prerequisites

To complete this tutorial, you need

- An Azure subscription. If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) before you begin.
- [Visual Studio Code](https://code.visualstudio.com/download), which you can download and use for free.
- The [Microsoft QDK for VS Code extension](https://marketplace.visualstudio.com/items?itemName=quantum.quantum-devkit-vscode).
- The [Azure CLI and the quantum CLI extension](xref:microsoft.quantum.install-qdk.overview.standalone#azure-cli-net-core-sdk-31-not-required).
- An Azure Quantum workspace with the **IonQ** provider enabled. For more information, see [Create an Azure Quantum workspace](xref:microsoft.quantum.how-to.workspace).


## Create a Q# project in Visual Studio Code

1. In VS Code open the **View** menu, and select **Command Palette**.

1. Type **Q#: Create New Project**.

1. Select **Standalone console application**.

1. Select a directory to hold your project, such as your home directory. Enter **QuantumRNG** as the project name, then select **Create Project**.

1. From the window that appears at the bottom, select **Open new project**.

1. You should see two files: the project file and **Program.qs**, which contains starter code. Open **Program.qs**.

1. Start by opening the **QuantumRNG.csproj** file and adding the `ExecutionTarget` property, which will give you design-time feedback on the compatibility of your program for IonQ's hardware.

```xml
<Project Sdk="Microsoft.Quantum.Sdk/0.17.2105143879">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp3.1</TargetFramework>
    <ExecutionTarget>ionq.qpu</ExecutionTarget>
  </PropertyGroup>
</Project>
```

1. Replace the contents of **Program.qs** with the program:

```qsharp
namespace QuantumRNG {
    open Microsoft.Quantum.Intrinsic;
    open Microsoft.Quantum.Measurement;
    open Microsoft.Quantum.Canon;

    @EntryPoint()
    operation GenerateRandomBits() : Result[] {
        use qubits = Qubit[4];
        ApplyToEach(H, qubits);
        return MultiM(qubits);
    }
}
```

> [!NOTE] 
> If you would like to learn more about this program code, see [Create your first Q# program by using the Quantum Development Kit](/learn/modules/qsharp-create-first-quantum-development-kit/).

## Prepare the Azure CLI

Next, we'll prepare your environment to run the program against the workspace you created.

1. Log in to Azure using your credentials. You'll get a list of subscriptions associated with your account.

   ```azurecli
   az login
   ```
   
1. Specify the subscription you want to use from those associated with your Azure account. You can also find your subscription ID in the overview of your workspace in Azure Portal.

   ```azurecli
   az account set -s <Your subscription ID>
   ```
   
1. Use `quantum workspace set` to select the workspace you created above
   as the default Workspace. Note that you also need to specify the resource
   group and the location you created it in:

   ```azurecli
   az quantum workspace set -g MyResourceGroup -w MyWorkspace -l MyLocation -o table
   ```

   ```output
    Location    Name         ProvisioningState    ResourceGroup    StorageAccount      Usable
    ----------  -----------  -------------------  ---------------  ------------------  --------
    MyLocation  MyWorkspace  Succeeded            MyResourceGroup  /subscriptions/...  Yes

   ```

1. In your workspace, there are different targets available from the
   providers that you added when you created the workspace. You can display a list of all
   the available targets with the command `az quantum target list -o table`:

   ```azurecli
   az quantum target list -o table
   ```

   Depending on the provider you selected, you will see:

   ```output
    Provider    Target-id                                       Status     Average Queue Time
    ----------  ----------------------------------------------  ---------  --------------------
    ionq        ionq.qpu                                        Available  0
    ionq        ionq.simulator                                  Available  0
    ```    

    > [!NOTE]
    > When you submit a job in Azure Quantum it will wait in a queue until the
    > provider is ready to run your program. The **Average Queue Time** column of
    > the target list command shows you how many seconds recently run jobs waited
    > in the queue. This can give you an idea of how long you might have to wait.

## Simulate the program in the IonQ provider

Before you run a program against real hardware, we recommend simulating it first (if possible, based on the number of qubits required) to help ensure that your algorithm is doing what you want. Fortunately, IonQ provides an idealized simulator that you can use.

> [!NOTE]
> You can also simulate Q# programs locally using the [Full State Simulator](xref:microsoft.quantum.machines.overview.full-state-simulator).

Run your program with `az quantum execute --target-id ionq.simulator -o table`. This command will compile your program, submit it to Azure Quantum, and wait until IonQ has finished simulating the program. Once it's done it will output a histogram which should look like the one below:

   ```azurecli
   az quantum execute --target-id ionq.simulator -o table
   ```

   ```output
   Result     Frequency
   ---------  -----------  -------------------------
   [0,0,0,0]  0.06250000   ▐█                      |
   [1,0,0,0]  0.06250000   ▐█                      |
   [0,1,0,0]  0.06250000   ▐█                      |
   [1,1,0,0]  0.06250000   ▐█                      |
   [0,0,1,0]  0.06250000   ▐█                      |
   [1,0,1,0]  0.06250000   ▐█                      |
   [0,1,1,0]  0.06250000   ▐█                      |
   [1,1,1,0]  0.06250000   ▐█                      |
   [0,0,0,1]  0.06250000   ▐█                      |
   [1,0,0,1]  0.06250000   ▐█                      |
   [0,1,0,1]  0.06250000   ▐█                      |
   [1,1,0,1]  0.06250000   ▐█                      |
   [0,0,1,1]  0.06250000   ▐█                      |
   [1,0,1,1]  0.06250000   ▐█                      |
   [0,1,1,1]  0.06250000   ▐█                      |
   [1,1,1,1]  0.06250000   ▐█                      |
   ```

This shows an equal frequency for each of the 16 possible states for measuring 4 qubits, which is what we expect from an idealized simulator! This means we're ready to run it on the QPU.

## Run the program on hardware

To run the program on hardware, we'll use the asynchronous job submission command `az quantum job submit`. Like the `execute` command this will compile and submit your program, but it won't wait until the execution is complete. We recommend this pattern for running against hardware, because you may need to wait a while for your job to finish. To get an idea of how long that may be, you can run `az quantum target list -o table` as described above. Depending on the provider you selected, you will see:


   ```azurecli
   az quantum job submit --target-id ionq.qpu -o table
   ```

   ```output
    Name        Id                                    Status    Target    Submission time
    ----------  ------------------------------------  --------  --------  ---------------------------------
    QuantumRNG  5aa8ce7a-25d2-44db-bbc3-87e48a97249c  Waiting   ionq.qpu  2020-10-22T22:41:27.8855301+00:00
   ```

The tables above show that your job has been submitted and is waiting for its turn to run. To check on the status, use the `az quantum job show` command, being sure to replace the `job-id` parameter with the Id output by the previous command, for example:

   ```azurecli
    az quantum job show -o table --job-id 5aa8ce7a-25d2-44db-bbc3-87e48a97249c 
   ```

   ```output
    Name        Id                                    Status    Target    Submission time
    ----------  ------------------------------------  --------  --------  ---------------------------------
    QuantumRNG  5aa8ce7a-25d2-44db-bbc3-87e48a97249c  Waiting   ionq.qpu  2020-10-22T22:41:27.8855301+00:00
   ```

Eventually, you will see the `Status` in the above table change to `Succeeded`. Once that's done you can get the results from the job by running `az quantum job output`:

   ```azurecli
   az quantum job output -o table --job-id 5aa8ce7a-25d2-44db-bbc3-87e48a97249c 
   ```

   ```output
   Result     Frequency
   ---------  -----------  -------------------------
   [0,0,0,0]  0.05200000   ▐█                      |
   [1,0,0,0]  0.07200000   ▐█                      |
   [0,1,0,0]  0.05000000   ▐█                      |
   [1,1,0,0]  0.06800000   ▐█                      |
   [0,0,1,0]  0.04600000   ▐█                      |
   [1,0,1,0]  0.06000000   ▐█                      |
   [0,1,1,0]  0.06400000   ▐█                      |
   [1,1,1,0]  0.07600000   ▐██                     |
   [0,0,0,1]  0.04800000   ▐█                      |
   [1,0,0,1]  0.06200000   ▐█                      |
   [0,1,0,1]  0.07400000   ▐█                      |
   [1,1,0,1]  0.08000000   ▐██                     |
   [0,0,1,1]  0.05800000   ▐█                      |
   [1,0,1,1]  0.06800000   ▐█                      |
   [0,1,1,1]  0.05200000   ▐█                      |
   [1,1,1,1]  0.07000000   ▐█                      |
   ```

The histogram you receive may be slightly different than the one above, but you should find that the states generally are observed with equal frequency.
