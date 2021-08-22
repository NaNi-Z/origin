#
# This image adds the repository link for Origin
# RPMs to allow downstream images to install the
# components using `yum'.
#
# The standard name for this image is openshift/origin-source
#
FROM centos:7

RUN rm -fr /etc/yum.repos.d/*
COPY *.repo /etc/yum.repos.d/
