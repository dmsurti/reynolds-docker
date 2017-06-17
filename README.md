This contains a Dockerfile to build a docker image for running the Blender app
with GUI and the reynolds-blender addon.

This repository also contains a cavity-tutorial directory which contains the
cavity case and a sample cavity.blend starter file with a 3D cube.

Important
---

This image currently runs only on Linux, tested on Ubuntu 17.04. As this
container runs a GUI app from within a Docker container, to run on MacOS, you
need to install Quartz, run X11, add your host to access control list and then
run the docker container. [This may help, for running on Mac
OS](http://sourabhbajaj.com/blog/2017/02/07/gui-applications-docker-mac/).

Hence, it is recommended to use Ubuntu for testing this docker imgae.

Steps
---

1. Build the docker image and copy the cavity-tutorial directory, which will be
   used in step 2, when running the container.

    `docker build -t blender .`
    
    `cp -r <path-to-this-cloned-repo-dir>/cavity-tutorial ~/`

   You can watch the instruction video for this step.

2. Run the docker container while sharing the copied `cavity-tutorial` with a
   bind volume and the X11 host to drive the Blender GUI from within the
   container.

    ```
    docker run -d -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix:rw \ # SHARE X11 HOSt
    -v ~/cavity-tutorial:/cavity-tutorial:rw \ # SHARE CAVITY CASE DIR
    blender
   ```

   You can watch the instruction video for this step.

3. Now you can generate the blockMeshDict, start OpenFoam, and run the solver
   from within blender. 

   Please watch the following instructional video to generate blockMeshDict.

   click the `Start Open Foam` and `Solve Open Foam` buttons in Blender to
   start openfoam and solve the case respectively.

4. You can now post process with `paraFoam` as such from the shared case
   directory on the host terminal, outside the container. This assumes you have
   installed `openfoam4` on the host.

   ```
   source /opt/openfoam4/etc/bashrc
   paraFoam
   ```
   You can watch the instruction video for this step.
