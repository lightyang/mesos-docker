# Mesos in Docker

## Usage
NOTE: On start up, the agent will kill all docker containers whose names start with `mesos-` and are
not recognized by it so we cannot name any container `mesos-...`.

To start a master:
```bash
docker run -d --name=my-mesos-master --net=host --restart=always \
  -v "$(pwd)/log/mesos:/var/log/mesos" \
  -v "$(pwd)/tmp/mesos:/var/tmp/mesos" \
  mansheng/mesos-master \
    --port=5050 \
    --zk=zk://localhost:2181/mesos \
    --quorum=1 \
    --log_dir=/var/log/mesos \
    --work_dir=/var/tmp/mesos
```

To start an agent:
```bash
docker run -d --name=my-mesos-agent --net=host --privileged --restart=always \
  -v "/usr/lib/libapparmor.so.1:/usr/lib/libapparmor.so.1" \
  -v "$(pwd)/log/mesos:/var/log/mesos" \
  -v "$(pwd)/tmp/mesos:/var/tmp/mesos" \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /cgroup:/cgroup \
  -v /sys:/sys \
  -v /usr/bin/docker:/usr/bin/docker \
  mansheng/mesos-agent \
    --port=5051 \
    --master=zk://localhost:2181/mesos \
    --switch_user=0 \
    --containerizers=docker,mesos \
    --log_dir=/var/log/mesos \
    --work_dir=/var/tmp/mesos \
    --docker_store_dir=/var/tmp/mesos/store/docker \
    --docker_volume_checkpoint_dir=/var/tmp/mesos/isolators/docker/volume \
    --fetcher_cache_dir=/var/tmp/mesos/fetch \
    --executor_environment_variables='{"LD_LIBRARY_PATH":"/usr/local/lib"}'
```
