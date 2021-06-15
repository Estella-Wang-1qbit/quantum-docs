---
author: george-moussa
description: Reference for azure.quantum.optimization.Job
ms.author: georgenm
ms.date: 02/01/2021
ms.service: azure-quantum
ms.subservice: optimization
ms.topic: reference
title: Quantum optimization job
uid: microsoft.quantum.optimization.job-reference
---

# Quantum optimization Job

```py
from azure.quantum.optimization import Job
```

## Job.get_results

Retrieves the job result (that is, the computed solution and cost). If the job has not
yet finished, blocks until it has.

```py
results = job.get_results()
print(results)

> {'version': '1.0', 'solutions': [{'configuration': {'1': 1, '0': 1, '2': -1, '3': 1}, 'cost': -23.0}]}
```

## Job.refresh

Refreshes the job's details by querying the workspace.

```py
job = workspace.get_job(jobId)
job.refresh()
print(job.id)

> 5d2f9cd70f55f149e3ed3aef
```

## Job.has_completed

Returns a boolean value indicating whether the job has finished (for example, the job is in a
[final state](xref:microsoft.quantum.azure-quantum-overview#monitoring-jobs)).

```py
job = workspace.get_job(jobId)
job.refresh()
print(job.has_completed())

> False
```

## Job.wait_until_completed

Keeps refreshing the job's details until it reaches a final state. For more information on job states, see [Azure Quantum Overview](xref:microsoft.quantum.azure-quantum-overview).

```py
job = workspace.get_job(jobId)
job.wait_until_completed()
print(job.has_completed())

> True
```