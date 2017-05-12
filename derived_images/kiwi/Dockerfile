FROM opensuse:42.1
MAINTAINER Bjoern Schilberg <bjoern@intevation.de>

# Latest KIWI packages
RUN zypper ar -f http://download.opensuse.org/repositories/Virtualization:/Appliances/openSUSE_Leap_42.1/ virtualization:appliances

# make sure the package repository is up to date
RUN zypper --non-interactive --gpg-auto-import-keys ref

# Install kiwi and remove the conflicting packages, otherwise zypper will stop here
RUN zypper in -y kiwi -libudev-mini1 -udev-mini

# Required to build SLES images on a recent systems
RUN zypper in -y  db45-utils

RUN zypper in -y sudo make

# Always start kiwi
ENTRYPOINT ["/usr/sbin/kiwi"]
