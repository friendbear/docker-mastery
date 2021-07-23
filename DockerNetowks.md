

Docker Network
=====

Docker Network: cmds
----
* ` docker inspect --format '{{ .NetworkSettings.IPAddress }}' webhost`

```sh
λ docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "f72ac65deaa1f24ec96398514c03fda16fc4a8ccd2f02ce1b2fc05cc9bcf3b5d",
        "Created": "2021-07-21T10:56:59.51548Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "457ffb149218906db8cccacb377afa1ef9c2cdfb05b0aec6e7c6ab54b945b0c7": {
                "Name": "webhost",
                "EndpointID": "52e0431da820a564cf1a7b07920ac69720ec3e0e7ed3685fa8278d748eb98e48",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```


* docker network ls
* docker network inspect
* docker network create --driver
* docker network connect
* docker network disconnect


Docker Networks: DNS
----

DNS NAME container name

```
docker network create my_app_net
docker network connect my_app_net nginx01
docker network connect my_app_net nginx02

docker exec -it nginx01 ping nginx02
```
![img](image/network-inspect-and-ping.png)


DNS Roud Robin Test
*****
```sh
C:\Applications\cmder                                                                                                                                
λ docker run -d --net dode --net-alias search elasticsearch:2                                                                                        
ed568524504c912a18979afbfb486ea4c755b8408e5b8547416633e795828a05                                                                                     
                                                                                                                                                     
C:\Applications\cmder                                                                                                                                
λ docker run -d --net dode --net-alias search elasticsearch:2                                                                                        
f03374c2dc44a68f5c7aee359770ff87ac155eecdceb4dd76797c571dddba35c                                                                                     
                                                                                                                                                     
C:\Applications\cmder                                                                                                                                
λ docker container ls                                                                                                                                
CONTAINER ID   IMAGE             COMMAND                  CREATED          STATUS          PORTS                               NAMES                 
f03374c2dc44   elasticsearch:2   "/docker-entrypoint.…"   18 seconds ago   Up 9 seconds    9200/tcp, 9300/tcp                  agitated_leakey       
ed568524504c   elasticsearch:2   "/docker-entrypoint.…"   25 seconds ago   Up 21 seconds   9200/tcp, 9300/tcp                  interesting_chatelet  
51734ad3b8c8   nginx             "/docker-entrypoint.…"   44 minutes ago   Up 44 minutes   80/tcp                              new_ngnx              
4427d21b8627   nginx             "/docker-entrypoint.…"   47 minutes ago   Up 47 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp   webhost               
                                                                                                                                                     
C:\Applications\cmder                                                                                                                                
λ docker run --rm --net dude alpine nslookup search                                                                                                  
docker: Error response from daemon: network dude not found.                                                                                          
                                                                                                                                                     
C:\Applications\cmder                                                                                                                                
λ docker run --rm --net dode alpine nslookup search                                                                                                  
Server:         127.0.0.11                                                                                                                           
Address:        127.0.0.11:53                                                                                                                        
                                                                                                                                                     
Non-authoritative answer:                                                                                                                            
*** Can't find search: No answer                                                                                                                     
                                                                                                                                                     
Non-authoritative answer:                                                                                                                            
Name:   search                                                                                                                                       
Address: 172.19.0.3                                                                                                                                  
Name:   search                                                                                                                                       
Address: 172.19.0.2

```
* GET elasticsearch:9200
   
   return rondom. Check `name.`
```sh
C:\Applications\cmder
λ docker run --rm --net dode centos curl -s search:9200
{
  "name" : "Nitro",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "WgyYSwDVTKa8PfvXvXobrQ",
  "version" : {
    "number" : "2.4.6",
    "build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
    "build_timestamp" : "2017-07-18T12:17:44Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.4"
  },
  "tagline" : "You Know, for Search"
}

C:\Applications\cmder
λ docker run --rm --net dode centos curl -s search:9200
{
  "name" : "John Proudstar",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "Wr9SEwBLRwiJ07QknsrUTQ",
  "version" : {
    "number" : "2.4.6",
    "build_hash" : "5376dca9f70f3abef96a77f4bb22720ace8240fd",
    "build_timestamp" : "2017-07-18T12:17:44Z",
    "build_snapshot" : false,
    "lucene_version" : "5.5.4"
  },
  "tagline" : "You Know, for Search"
}

```