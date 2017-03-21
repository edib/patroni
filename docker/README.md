# Patroni Dockerfile
You can run Patroni in a docker container using this Dockerfile, or by using one of the Docker image at

    https://hub.docker.com/r/kokdemir/patroni/

This Dockerfile is meant in aiding development of Patroni and quick testing of features. It is not a production-worthy
Dockerfile

# Examples

## Standalone Patroni

    docker run -d kokdemir/patroni

## Multiple Patroni's communicating with a standalone etcd inside Docker

Basically what you would do would be: 

* Run 1 container which provides etcd

        docker run -d <IMAGE> --etcd-only

* Run n containers running Patroni, passing the `--etcd` option to the `docker run` command

        docker run -d <IMAGE> --etcd=<IP FROM etcd CONTAINER:PORT>

To automate this you can run the following script:

    dev_patroni_cluster.sh [OPTIONS]

    Options:

        --image     IMAGE    The Docker image to use for the cluster
        --members   INT      The number of members for the cluster
        --name      NAME     The name of the new cluster

Example session:
# ./dev_patroni_cluster.sh --image kokdemir/patroni --members=3 --name=demo
Started container demo_etcd, ip=172.17.0.7
Started container demo_postgres1, ip=172.17.0.8
Started container demo_postgres2, ip=172.17.0.9
Started container demo_postgres3, ip=172.17.0.10
Started container demo_haproxy, ip=172.17.0.11
