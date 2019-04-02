# Latest version of centos
FROM centos:centos7.6.1810

MAINTAINER Dzmitry Kuzmin <kuzmin.mail@gmail.com>

LABEL ansible_version="v2.7.9"
LABEL centos_version="7.6.1810"

RUN yum clean all && \
    yum -y update && \
    yum -y install epel-release && \
    yum -y install git make gcc python-abi python-crypto \
        python-httplib2 python-jinja2 python-keyczar \
        python-paramiko python-setuptools python-six \
        python2-jmespath python-pip sshpass \
        python-devel openssl-devel libselinux-python
RUN pip install --upgrade pip && \
    pip install packaging && \
    pip install --ignore-installed requests && \
    pip install molecule==2.20.0
RUN mkdir /etc/ansible/
RUN echo -e '[local]\nlocalhost\n' > /etc/ansible/hosts
RUN echo -e '[defaults]\nstdout_callback = yaml\n' > /etc/ansible/ansible.cfg
RUN mkdir /opt/ansible/
RUN git clone -b 'v2.7.9' --single-branch --depth 1  http://github.com/ansible/ansible.git /opt/ansible/ansible
WORKDIR /opt/ansible/ansible
RUN make && \
    make install
ENV PATH /opt/ansible/ansible/bin:/bin:/usr/bin:/sbin:/usr/sbin
ENV PYTHONPATH /opt/ansible/ansible/lib
ENV ANSIBLE_LIBRARY /opt/ansible/ansible/library