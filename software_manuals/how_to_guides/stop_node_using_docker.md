# How To Stop A Node Using Docker

### Goal
Correctly stop a node using docker.

### Steps

Update your doker image:
```sh
$ sudo docker pull cyberway/cyberway:v2.1.0
```

Pull out the current docker-compose.yml file:
```sh
$ sudo curl https://raw.githubusercontent.com/cyberway/cyberway.launch/master/docker-compose.yml --output /var/lib/cyberway/docker-compose.yml
```

Tell docker-compose to update containers following the directions given in docker-compose.yml:
```sh
$ cd /var/lib/cyberway
$ sudo docker-compose up -t 120 –d
```

### Useful links
  * [Configuring the Docker Image](https://docs.cyberway.io/validators/testnet_installation/docker_configuration)
  * [Commands Applicable to Any Kind of Container](https://docs.cyberway.io/validators/testnet_installation/main_commands)
