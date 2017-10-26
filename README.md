# HAProxy-Docker-Sample

After following the steps below you should be able to browse to `site1.org` and `site2.org` in your default web browser and see the HAProxy woking.

## Proxy Network

A overlay network has to be created first using the following command:

```
docker network create --driver overlay --scope swarm proxy
```

## Deploy Proxy

```
docker stack deploy -c proxy\docker-compose.haproxy.yml proxy
```

## Deploy visualizer

```
docker stack deploy -c viz\docker-compose.viz.yml viz
```

## Deploy Site 1

```
docker stack deploy -c site1\docker-compose.web.yml site1
```

## Deploy Site 2

```
docker stack deploy -c site1\docker-compose.web.yml site2
```

## (local dev only) Update Hosts file

Add the host `site1.org` and `site2.org` to your hosts file.

On windows 10 the hosts files is under: `C:\Windows\System32\drivers\etc`

On a mac the hosts file is under: `/private/etc`

Add the following lines.

```
127.0.0.1       site1.org
127.0.0.1       site2.org
127.0.0.1       viz.org
```

## Stats page

HAProxy offers a nice stats page. Open your browser and navigate to `localhost:1936`. It uses basic auth for seccurity. Default username and password is `stats:stats`

## Scaling

It is now possible to scale your webservice to as many containers as needed. HAProxy will take care of load balancing all of them.

```
docker service scale site1_app=5
```
or
```
docker service scale site2_app=3
```
