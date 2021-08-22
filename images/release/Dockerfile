#
# This is the image that controls the standard build environment for releasing OpenShift Origin.
# It is responsible for performing a cross platform build of OpenShift.
#
# The standard name for this image is openshift/origin-release:go-1.12
#

FROM openshift/origin-base

ENV VERSION=1.12.17 \
    GOARM=5 \
    GOPATH=/go \
    GOROOT=/usr/local/go \
    OS_VERSION_FILE=/go/src/github.com/openshift/origin/os-version-defs
ENV PATH=$PATH:$GOROOT/bin:$GOPATH/bin

COPY rpms /tmp/rpms
RUN INSTALL_PKGS="bc bind-utils bsdtar bzr ceph-common createrepo device-mapper device-mapper-persistent-data e2fsprogs ethtool file findutils gcc git glibc-static glib2-devel gpgme gpgme-devel hostname iptables jq krb5-devel libassuan libassuan-devel libseccomp-devel libvirt-devel lsof make mercurial nmap-ncat openssl protobuf-compiler protobuf-devel rsync socat systemd-devel sysvinit-tools tar tito tree util-linux wget which xfsprogs zip" && \
    yum install -y $INSTALL_PKGS /tmp/rpms/*.rpm && \
    rpm -V $INSTALL_PKGS && \
    yum clean all
COPY go1.12.17.src.tar.gz go1.4-bootstrap-20171003.tar.gz /opt/
RUN tar -xvf /opt/go1.4-bootstrap-20171003.tar.gz -C /root && mv /root/go /root/go1.4 && cd /root/go1.4/src/ && CGO_ENABLED=0 ./make.bash
RUN tar -xvf /opt/go1.12.17.src.tar.gz -C /usr/local/ && cd /usr/local/go/src/ && ./all.bash
RUN mkdir -p /go/src/
COPY src /go/src/
RUN go install github.com/openshift/imagebuilder/cmd/imagebuilder
RUN touch /os-build-image && \
    git config --system user.name origin-release-container && \
    git config --system user.email none@nowhere.com

WORKDIR /go/src/github.com/openshift/origin
LABEL io.k8s.display-name="OpenShift Origin Release Environment (golang-$VERSION)" \
      io.k8s.description="This is the standard release image for OpenShift Origin and contains the necessary build tools to build the platform."
