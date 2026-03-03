---
title: OpenFOAM Workflow Example
toc: true
---

OpenFOAM is a popular engineering application toolbox. It's open source and is used for simulating fluid flow around objects. This section describes the different installation process available and how to run some basic jobs. Some parts in this section use the [Flight Silo]({{< relref "knowledge/flight-environment/use-flight/flight-user-suite/flight-silo.md" >}}) tool to download necessary files.

## Install OpenFOAM

There are 3 different ways you can install OpenFOAM, each with varying levels of time to setup and complexity. The *Basic* setup is the fastest and integrates best with the Flight User Suite. The *Advanced* setup is for users that want more control, and can take over an hour on a 16 core machine.

### Basic 

1. Download and install OpenFlight OpenFOAM software using [Flight Silo]({{< relref "knowledge/flight-environment/use-flight/flight-user-suite/flight-silo.md" >}})
    ```bash
    flight silo software pull OpenFOAM 22.12 --repo openflight
    ```

1. Install dependencies for visualisation.
    ```bash
    sudo dnf install -y paraview
    ```

1. Install dependencies on all nodes.
    ```bash
    pdsh -g all 'sudo dnf install -y openmpi'
    ```

1. Source the OpenFOAM environment and run a basic test. Make sure to change the source file path to the location of your installation. **Note that the `module` command will not be available to a shell session existing before the installation, logout and back in for it to be present**
    ```bash
    module load mpi
    source apps/OpenFOAM/22.12/etc/bashrc
    foamTestTutorial -full incompressible/simpleFoam/pitzDaily
    ```

### Advanced

Firstly, become the root user, the added permissions are necessary for setup.
```bash
sudo su -
```

1. Install prerequisites
    ```bash
    dnf install gcc cmake boost fftw-devel paraview-devel paraview-openmpi-devel openmpi-devel flex m4 qt5-devel
    ```
2. Make a directory for OpenFOAM in shared storage, so that the OpenFOAM build is available to all nodes in the cluster.
    ```bash
    cd /opt/apps
    mkdir OpenFOAM
    ```
3. Obtain OpenFOAM Source.
    ```bash
    flight silo file pull openflight:openfoam/OpenFOAM-v2212.tar.gz
    tar xf OpenFOAM-v2212.tgz
    ```
4. Compile it.
    ```bash
    cd OpenFOAM-v2212
    module load mpi
    source etc/bashrc

    foamSystemCheck # Check whether expected to work

    ./Allwmake -j -s -q -l # Compile on all cores
    ```
5. Install dependencies for visualisation.
    ```bash
    dnf install -y paraview
    ```
6. Install dependencies on all nodes.
    ```bash
    pdsh -g all 'dnf install -y openmpi'
    ```
7. Test that the build works.
    ```bash
    foamTestTutorial -full incompressible/simpleFoam/pitzDaily
    ```

> From this point being the root user is no longer necessary.

### Legacy

1. Enable copr for OpenFOAM.
    ```bash
    sudo dnf -y copr enable openfoam/openfoam
    ```

1. Install OpenFOAM, the OpenFOAM version selector, the additional sub-packages and paraview.
    ```bash
    sudo dnf install -y openfoam-selector
    sudo dnf install -y openfoam
    sudo dnf install -y openfoam2212-default
    sudo dnf install -y paraview
    ```

1. Ensure that the correct version will be used with:
    ```bash
    openfoam-selector --set openfoam2212
    ```

1. Refresh the shell by logging out and back in to make openfoam commands accessible.

> Make sure to do the above installation steps on **all** nodes in the cluster.

1. Check an OpenFOAM command can be seen by viewing the help page:
    ```bash
    icoFoam -help
    ```

## Simple Workflow Example

For this example, your environment will need to have a job scheduler such as [Slurm]({{< relref "knowledge/hpc-environment-basics/slurm/_index.md" >}})

### Run Job

1. Download a job script to the working directory. This script assumes that it is being launched from the home directory, change it if that is not the case. It also cannot be run as the root user.

    ```bash
    flight silo file pull openflight:openfoam/cavity-example.sh
    ```

2. Submit the job to the queue system:
    ```bash
    sbatch cavity-example.sh
    ```

3. Check that the job is running (it may take some time to complete):
    ```bash
    squeue
    ```

### View Results

Once the cavity job has finished running, the results can be visualised through a desktop session.

1. Connect to VNC or web-suite desktop (For more information on launching desktops, see the [Flight Desktop section]({{< relref "knowledge/flight-environment/use-flight/flight-user-suite/flight-desktop.md" >}})).

2. Navigate to the cavity directory and launch the paraFoam viewer:
    ```bash
    cd cavity
    paraFoam
    ```

3. In the window that opens, scroll down to "Mesh parts" and select all the boxes, then click apply
    ![](/images/knowledge/hpc-workflows/openfoam_parafoam_1.png)

4. Next, change the display to 'U' in the drop-down menu, click play to change the timestamp and then click and drag to rotate the subject
    ![](/images/knowledge/hpc-workflows/openfoam_parafoam_2.png)

## MPI Workflow Example

For this example, your environment will need to have a job scheduler such as [Slurm]({{< relref "knowledge/hpc-environment-basics/slurm/_index.md" >}})

### Prepare Job

1. Start by downloading this job directory in a location of your choice.
    ```bash
    flight silo file pull openflight:openfoam/motorBike.tar.gz
    ```

2. Decompress it, which will make a directory called `motorBike`.
    ```bash
    tar xf motorBike.tar.gz
    ```

3. Update the job configuration.
    1. Open `motorBike/system/decomposeParDict` and set:
        1. `X` to the number of processes to run across.
        1. `A B C` to be numbers such that `A`*`B`\*`C`=`X`

4. Update the job script.
    1. Open `motorBike/motorbike-example.sh`.
        1. Set `-n X` to the number of processes to run across.
        1. Set `source` to point to the location of OpenFOAM.
        1. Set `$HOME/motorBike` to the location of the motorBike job.

### Run Job

1. Submit the job script to the scheduler.
    ```bash
    sbatch motorBike/motorbike-example.sh
    ```

### View Results

Once the job has finished running, the results can be visualised through a desktop session.

1. Connect to VNC or web-suite desktop (For more information on launching desktops, see the [Flight Desktop section]({{< relref "knowledge/flight-environment/use-flight/flight-user-suite/flight-desktop.md" >}})).

2. Open a terminal and navigate to the job directory.

3. Launch Paraview
    ```bash
    source /opt/apps/OpenFOAM/OpenFOAM-v2212/etc/bashrc
    paraFoam
    ```

4. In the window that opens, go to the bottom left and scroll down to "Mesh parts". Select all the boxes  except `frontAndBack`, `inlet`, `internalMesh`, `outlet` and `upperWall`, and then click apply.

    ![](/images/knowledge/hpc-workflows/openfoam_parafoam_motorbike_1.png)

5. You should now see a motorbike along with a rider and the forces visualised on them!

    ![](/images/knowledge/hpc-workflows/openfoam_parafoam_motorbike_2.png)
