docker-biohadoop
================

A Dockerfile that installs Apache Hadoop 2.5.0, Apache Oozie 4.0.1 and Apache ZooKeeper 3.4.6 as a cluster environment.

Based on sequenceiq/hadoop-docker (https://index.docker.io/u/sequenceiq/hadoop-docker/)

## Installation

```
$ git clone https://github.com/gappc/docker-biohadoop.git
$ cd docker-biohadoop
$ sudo docker build -t="docker-biohadoop" .
```

## Usage
docker-biohadoop provides two scripts, located in the scripts directory, that can be used to start and stop docker-biohadoop instances. Those scripts need to be executable:

```
$ chmod +x scripts/*.sh
```

### docker-run-hadoop.sh (depends on gnome-terminal)
Use this script to start a number of Hadoop instances. It takes the number of slaves (nr-of-slaves) as argument. The script starts one Docker container as Hadoop master and additional nr-of-slaves Docker containers as Hadoop slaves.

e.g.
```
$ scripts/docker-run-hadoop.sh 2
```
starts one Docker container as Hadoop master and 2 Docker containers as Hadoop slaves. Note that also the master is used for computational purposes. Therefor, Hadoop has 3 machines for computation with the settings above.

After invoking docker-run-hadoop.sh, a gnome-terminal is started for every Docker container. The master containers terminal has a red color, the slaves terminals are yellow. The master container starts the Hadoop environment, which may take some time (depending on the hardware and the number of slaves). After this initialisation, the hadoop cluster is ready for usage. Try to invoke the command `jps` on all running containers to look if Hadoop is running

```
$ jps
```

On the master node, it should output:
* `DataNode`
* `JobHistoryServer`
* `NodeManager`
* `NameNode`
* `QuorumPeerMain`
* `ResourceManager`
* `SecondaryNameNode`

On the slave nodes it should output
* `DataNode`
* `NodeManager`.

### docker-stop-all.sh
Stops all running Docker containers and removes their interfaces from host.

```
$ scripts/docker-stop-all.sh
```

## SSH access
The master node is accessible with user `root`, a password is generated on each startup and printed on the terminal. Consider adding your SSH key to the Dockerfile if you are going to use docker-biohadoop often.
