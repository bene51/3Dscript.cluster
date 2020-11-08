3Dscript.cluster
================

Fiji plugins for rendering 3D/4D animations via [3Dscript](https://bene51.github.com/3Dscript) on a HPC cluster. The script in this repository was implemented for the [Torque job management system](https://adaptivecomputing.com/), to be run on the [Emmy compute cluster of the University of Erlangen](https://www.anleitungen.rrze.fau.de/hpc/emmy-cluster/). Still, I believe it provides a good starting point to create similar scripts for other HPC environments.

Preparation
-----------
* Log into the frontend of your HPC system
* Download and extract Fiji:
    ```
    wget -q https://downloads.imagej.net/fiji/latest/fiji-linux64.zip &&
    unzip -q fiji-linux64.zip &&
    rm fiji-linux64.zip &&
    cd Fiji.app &&
    ./ImageJ-linux64 --update add-update-site 3Dscript "https://romulus.oice.uni-erlangen.de/updatesite/" &&
    ./ImageJ-linux64 --update add-update-site 3Dscript.server "https://romulus.oice.uni-erlangen.de/imagej/updatesites/3Dscript-server/" &&
    ./ImageJ-linux64 --update update &&
    cd ..
    ```
* Download and extract [FFmpeg](https://ffmpeg.org/):
    ```
    wget https://johnvansickle.com/ffmpeg/builds/ffmpeg-git-amd64-static.tar.xz &&
    tar -xf ffmpeg-git-amd64-static.tar.xz &&
    mv ffmpeg-git-20200324-amd64-static ffmpeg &&
    rm ffmpeg-git-amd64-static.tar.xz
    ```

Submitting a job
----------------
* Edit 3Dscript.bsh for your needs, in particular:
    * Update the OMERO host name and the corresponding user credentials so that the cluster nodes can retrieve the input data
    * Specify the OMERO image IDs that should be rendered
    * Specify the animation script
    * Adjust the size of the job array to submit to the number of images, by replacing the line `#PBS –t 0-17` with `#PBS –t 0-<nImages – 1>`
* Submit the script to the Torque job management system:
    ```
    qsub 3Dscript.bsh`
    ```
* Check the progress:
    ```
    qsub –t
    ```

The result videos will automatically be uploaded to OMERO as an attachment to the input images.

Links
-----
More information about 3Dscript:
* https://bene51.github.io/3Dscript
* https://github.com/bene51/3Dscript

Running 3Dscript.server as a Fiji plugin:
* https://github.com/bene51/3Dscript.server

Running 3Dscript.server as OMERO.web app:
* https://github.com/bene51/omero_3Dscript

Publication:
------------
Schmid, B.; Tripal, P. & Fraa&szlig;, T. et al. (2019), "3Dscript: animating 3D/4D microscopy data using a natural-language-based syntax", _Nature methods_ **16(4)**: 278-280, PMID 30886414.


