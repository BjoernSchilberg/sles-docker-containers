# How to create docker image based on SLES12

0. Download required ISOs

    http://download.novell.com/ (SLES12, + SDK if you need packages from it)

Put the ISO files in the SLES12/ folder.

1. Create Kiwi image for docker

        cd ../derived_images/kiwi
        sudo docker build -t kiwi .

2. Make sure your `config.xml` file is up to date regarding repo, maintainer, etc

3. Start kiwi container and build image:

        cd SLES12/
        sudo docker run -it --entrypoint /bin/bash --privileged -v $(pwd):/tmp/workspace -w /tmp/workspace kiwi
        zypper in -y sudo make
        mount -o loop SLES12.iso /mnt
        make build

5. Copy the resulting tarball into the host and import it to docker

        sudo docker cp CONTAINER_ID:/tmp/SLES11_root_result/sles-12-docker-guest-docker.x86_64-1.0.0.tar.xz .
        sudo docker import - sles:12 < sles-12-docker-guest-docker.x86_64-1.0.0.tar.xz

6. [optional] Copy ISO content to serve as local zypper repositories (by default there is none)

Note: this adds ~3.5GB to the image.

    ID=$(sudo docker run -d --privileged -v $(pwd):/workspace sles:12 bash -x -c 'mkdir -p /mnt/{core-1,sdk-1} && mkdir -p /repos && mount -o loop /workspace/SLES12-DVD1.iso /mnt/core-1 && cp -r /mnt/core-1 /repos/ && mount -o loop /workspace/SLES12-SDK-DVD1.iso /mnt/sdk-1 && cp -r /mnt/sdk-1 /repos/')
    sudo docker attach "$ID"
    sudo docker commit "$ID" sles:12-with-repo
    sudo docker save sles:12-with-repo | gzip > sles12-with-repo.tar.gz
    # later: gunzip sles12-with-repo.tar.gz | sudo docker load

7. Profit!
