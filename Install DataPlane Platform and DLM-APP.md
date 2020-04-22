# DataPlane app and DLM app setup

## 1. Install and setup docker

### Remove old versions
```
yum remove docker \
       docker-client \
       docker-client-latest \
       docker-common \
       docker-latest \
       docker-latest-logrotate \
       docker-logrotate \
       docker-engine
```

### Setup the repo
```
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
### Install Docker engine
```
yum install docker-ce docker-ce-cli containerd.io -y
```
### Start Docker and verify if Docker engine is running
```
systemctl start docker
docker run hello-world
```

## 2. Install and setup DP-APP and DLM-APP

### Disable SELINUX
```
setenforce 0
sed -i 's/^SELINUX=.*/SELINUX=disabled/g' /etc/sysconfig/selinux
```

### Fetch and install the DP-APP tarball
1. Get the tarball from here: https://hortonworks.jira.com/browse/BUG-118420
```
cd /opt
wget http://downloads-hortonworks.akamaized.net/DP/1.2.2.0-9/centos7/DP-1.2.2.0-9-centos7-rpm.tar.gz
```
2. Extract the tarball and install the rpm packages
```
tar xf DP-1.2.2.0-9-centos7-rpm.tar.gz
cd /opt/DP/centos7/1.2.2.0-9/dp-select
rpm -ivh dp-select-1.2.2.0-9.noarch.rpm
cd /opt/DP/centos7/1.2.2.0-9/dp_core
rpm -ivh dp-core_1_2_2_0_9-1.7.0.1.2.2.0-9.noarch.rpm
```

### Fetch and install the DLM-APP tarball
1. Get the tarball from here: https://jira.cloudera.com/browse/RELENG-5681
```
cd /opt
wget http://downloads-hortonworks.akamaized.net/DLM-APP/1.5.0.0-43/DLM-APP-1.5.0.0-43-centos7-rpm.tar.gz
```
2. Extract the tarball and install the rpm packages
```
tar xf DLM-APP-1.5.0.0-43-centos7-rpm.tar.gz
cd /opt/DLM-APP/centos7/1.5.0.0-43/dlm-app-select/
rpm -ivh dlm-app-select-1.5.0.0-43.noarch.rpm
cd ../dlm_app
rpm -ivh dlm-app_1_5_0_0_43-1.7.0.1.5.0.0-43.noarch.rpm
```

### Deploy DataPlane service
```
cd /usr/dp/current/core/bin/
./dpdeploy load
./dpdeploy.sh load
./dpdeploy.sh init --all
```

### Deploy DLM-APP service
```
cd /usr/dlm-app/1.5.0.0-43/apps/dlm/bin/
./dlmdeploy.sh load
./dlmdeploy.sh init
```
