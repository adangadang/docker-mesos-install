# docker-mesos-install

docker install 

最新  curl -sSL https://get.docker.com/ |  sh    或 yum install docker 

#mesos marathon  zookeeper  install    # master install 

rpm -Uvh http://repos.mesosphere.io/el/7/noarch/RPMS/mesosphere-el-repo-7-2.noarch.rpm

yum install mesos

systemctl  disable mesos-slave
systemctl  enable mesos-master
systemctl  start mesos-master
[root@local24 ~]# more /etc/mesos/zk 
zk://localhost:2181/mesos
[root@local24 ~]# more /etc/mesos-master/hostname 
10.15.206.24
[root@local24 ~]# more /etc/mesos-master/ip
10.15.206.24

yum install  mesosphere-zookeeper
systemctl  enable zookeeper
systemctl  stop zookeeper
systemctl  start zookeeper

yum install marathon
systemctl  enable marathon
systemctl  stop marathon
systemctl  start marathon


 # agent install 
 rpm -Uvh http://repos.mesosphere.io/el/7/noarch/RPMS/mesosphere-el-repo-7-2.noarch.rpm
yum install mesos
systemctl  enable mesos-slave
systemctl  disable mesos-master
systemctl  start mesos-slave

more /etc/mesos/zk 
zk://10.15.206.24:2181/mesos
more /etc/mesos-slave/ip
10.15.206.23
more /etc/mesos-slave/hostname
10.15.206.23

# marathon-lb 在centos下性能差，单进程只有8k/s左右 centos版的haproxy在2w/s左右
 docker run  --rm --name lb -e PORTS=9090 --net=host  mesosphere/marathon-lb sse --marathon http://10.15.206.24:8080 --group external
 
 
 
 docker run -d --net=host -v "/usr/local/mesos-dns/config.json:/config.json" -v "$(pwd)/logs:/tmp" mesosphere/mesos-dns:0.5.2 /usr/bin/mesos-dns -v=2 -config=/config.json
 
 [root@local24 ~]# more /usr/local/mesos-dns/config.json

{
  "zk": "zk://10.15.206.24:2181/mesos",
  "refreshSeconds": 60,
  "ttl": 60,
  "domain": "mesos",
  "port": 53,
  "resolvers": ["10.15.201.223","10.15.201.254"],
  "timeout": 5,
  "email": "root.mesos-dns.mesos"
}
[root@local24 ~]# 
