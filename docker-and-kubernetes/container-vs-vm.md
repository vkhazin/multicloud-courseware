# Container vs. VM

* The VM model blends an application, a full guest OS, and a disk emulation into the image
* Container model uses just the application's dependencies and runs them directly on the host OS
* Container model reduces the overhead associated with starting and running instances
* The same host can host 10-15 VM's compared to dozens or hundreds of containers
* The isolation from OS kernel provided by containers is less robust than that of a hypervisor for virtual machines

  ![](../media/container-vs-vm.jpg)
