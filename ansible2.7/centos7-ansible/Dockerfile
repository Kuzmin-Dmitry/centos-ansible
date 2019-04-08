# Latest version of centos
FROM centos:centos7.6.1810

MAINTAINER Dzmitry Kuzmin <kuzmin.mail@gmail.com>

LABEL ansible_version="v2.7.9"
LABEL centos_version="7.6.1810"


# Prepare OS
RUN yum makecache fast && yum update -y && \
    yum -y install epel-release && \
    yum -y install git make gcc python-abi python-crypto \
        python-httplib2 python-jinja2 python-keyczar \
        python-paramiko python-setuptools python-six \
        python2-jmespath python-pip sshpass \
        python-devel openssl-devel libselinux-python
RUN pip install --upgrade pip && \
    pip install packaging

# Install Ansible
RUN mkdir /etc/ansible/
RUN mkdir /opt/ansible/
RUN git clone -b 'v2.7.9' --single-branch --depth 1  http://github.com/ansible/ansible.git /opt/ansible/ansible

WORKDIR /opt/ansible/ansible

RUN make && \
    make install

RUN echo -e '[local]\nlocalhost\n' > /etc/ansible/hosts
RUN echo -e '[defaults]\nstdout_callback = yaml\n' > /etc/ansible/ansible.cfg

ENV PATH /opt/ansible/ansible/bin:/bin:/usr/bin:/sbin:/usr/sbin
ENV PYTHONPATH /opt/ansible/ansible/lib
ENV ANSIBLE_LIBRARY /opt/ansible/ansible/library

# Configure systemd support
RUN yum -y install systemd; yum clean all && \
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done) && \
rm -f /lib/systemd/system/multi-user.target.wants/* && \
rm -f /etc/systemd/system/*.wants/* && \
rm -f /lib/systemd/system/local-fs.target.wants/* && \
rm -f /lib/systemd/system/sockets.target.wants/*udev* && \ 
rm -f /lib/systemd/system/sockets.target.wants/*initctl* && \
rm -f /lib/systemd/system/basic.target.wants/* && \
rm -f /lib/systemd/system/anaconda.target.wants/*

VOLUME [ "/sys/fs/cgroup" ]

ENTRYPOINT ["/usr/sbin/init"]