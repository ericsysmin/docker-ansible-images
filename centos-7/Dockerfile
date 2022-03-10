FROM centos:7
ENV container=docker
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

# Install systemd -- See https://hub.docker.com/_/centos/
RUN yum -y update; yum clean all; \
(cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

RUN yum makecache fast \
  && yum -y install deltarpm epel-release initscripts \
  && yum -y update \
  && yum -y groupinstall "Development Tools" \
  && yum -y install wget sudo which gcc openssl-devel libffi-devel bzip2-devel \
  && yum clean all

RUN wget https://www.python.org/ftp/python/3.9.10/Python-3.9.10.tgz \
  && tar xvf Python-3.9.10.tgz \
  && cd Python-3.9*/ \
  && ./configure --enable-optimizations \
  && make altinstall \
  && cd ~ && rm -rf Python-3.9*

RUN pip3.9 install ansible

# Disable requiretty.
RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

# Install Ansible inventory file.
RUN mkdir -p /etc/ansible
RUN echo -e '[local]\nlocalhost ansible_connection=local' > /etc/ansible/hosts

VOLUME ["/sys/fs/cgroup"]
CMD ["/usr/lib/systemd/systemd"]
