# HPC on Azure

At moment of writing there are two main options for HPC on Azure: [CycleCloud](https://learn.microsoft.com/en-us/azure/cyclecloud/?view=cyclecloud-8) and [Azure Batch](https://learn.microsoft.com/en-us/azure/batch/). Both are tools for managing HPC environments on Azure, and both integrate well with the rest of the Azure stack like Azure Monitor and Azure Cost Management tools.

🔎 Let's take a look at them separately first, and then compare for differences and what use cases they're best applicable for.

## CycleCloud

Is targeted at HPC administrators and users with a specific scheduler in mind. E.g. Slurm, PBSPro, Grid Engine and LSF which are supported out-of-the-box. CycleCloud deploys autoscaling plugins on top of these supported schedulers.
*There is no job scheduling functionality in CycleCloud.* It also does not dictate cluster topology. Through templates you can get HPC systems up and running quickly and these can be customized to meet your requirements.

Your HPC environments can be managed through a single management plane.

CycleCloud is ideal for organizations who have experience with operating HPC environments on-prem and specific schedulers. *It is therefore also especially suited for hybrid use cases* (where on-prem and cloud are mixed).

**How it works:**
CycleCloud is an installable web application that you can run on-prem or in an Azure VM. It can then be configured to use compute and data resources from your subscription. [Templates](https://learn.microsoft.com/en-us/azure/cyclecloud/download-cluster-templates?view=cyclecloud-8) for schedulers and clusters are available for quick configuration.

Once a cluster is created it is automagically configured to autoscale by default.

## Azure Batch

Works well with intrinsically parallel workloads and HPC batch jobs. These are workloads that have applications running independently, with each instance completing part of the work at the same time. These can run at a large scale, determined by the amount of compute resources available.
If you need to run tightly coupled workloads, where the applications need to communicate with each other, using [Microsoft MPI](https://learn.microsoft.com/en-us/message-passing-interface/microsoft-mpi) or Intel MPI.
LAstly it supports large-scale [rendering workloads](https://learn.microsoft.com/en-us/azure/batch/batch-rendering-service).

So it offers scheduler-as-a-service, which means you don't have to install, manage or scale cluster or job scheduler software.

**How it works:**
Batch creates (and manages) a *pool* of compute nodes. These are VM's on which your applications are installed that you want to run. It then schedules *jobs* to run on the nodes.

You can interact with Azure Batch through [CLI](https://learn.microsoft.com/en-us/azure/batch/quick-create-cli), [.NET API](https://learn.microsoft.com/en-us/azure/batch/quick-run-dotnet), [Python API](https://learn.microsoft.com/en-us/azure/batch/quick-run-python) or the [Azure Portal](https://learn.microsoft.com/en-us/azure/batch/quick-create-portal).
There are multiple libraries available for Batch as well.

You can track the progress through logs and metrics and view them on your observability tool of choice. Examples on Azure are:

- [Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/overview)
- [Azure Batch Explorer](https://azure.github.io/BatchExplorer/)

## 👯 Comparing CycleCloud and Batch

They are sister products of each other.

CycleCloud is basically the on-prem way of HPC on the cloud. So if you're used to on-prem way of working, or have hybrid workloads then this makes sense.

Batch is a more modern way of high performance computing. It is a bit more flexible in terms of API references and has some automation built-in. Most importantly, it offers a scheduler-as-a-service.

For both there are options around containerization (see [references below]()).

You can calculate the costs for each in the [pricing calculator](https://azure.microsoft.com/pricing/calculator/), based on your expected workloads.

### References

- [Overview of HPC on Azure](https://learn.microsoft.com/en-us/azure/architecture/topics/high-performance-computing)

**CycleCloud:**

- [Docs](https://learn.microsoft.com/en-us/azure/cyclecloud/overview?view=cyclecloud-8)
- [CycleCloud scheduling concepts](https://learn.microsoft.com/en-us/azure/cyclecloud/concepts/scheduling?source=recommendations)
- [HPC architectures: best practices](https://learn.microsoft.com/en-us/azure/architecture/topics/high-performance-computing?source=recommendations)
- [Running CycleCloud in a container instance](https://learn.microsoft.com/en-us/azure/cyclecloud/how-to/run-in-container?view=cyclecloud-8)

**Azure Batch**:

- [Docs](https://learn.microsoft.com/en-us/azure/batch/)
- [Supported VM sizes](https://learn.microsoft.com/en-us/azure/batch/batch-pool-vm-sizes)
- [Running container applications](https://learn.microsoft.com/en-us/azure/batch/batch-docker-container-workloads)