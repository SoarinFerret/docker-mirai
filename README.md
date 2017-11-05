# Mirai for Docker
Create your own Mirai botnet using Docker. 

## Pre-compiled Binaries
For use with Ubuntu 14.04 x64. For the source code, please refer to: [mirai](https://github.com/lejolly/mirai)

## Prerequisites
1. Docker Swarm

## Instructions
1. Clone this repository. 
2. In each of the `docker-bot` and `docker-cnc` directories, build the corresponding image
	- e.g. for the cnc: `$ sudo docker build -t mirai-cnc .`
	- e.g. for the bot: `$ sudo docker build -t mirai-bot .`
3. Create the botnet network
	- `$docker network create --subnet 172.20.0.0/16 --gateway 172.20.0.1 -d overlay -o com.docker.network.bridge.enable_icc:true botnet`
4. Start the cnc service
	- `$ docker service create --name cnc --mode replicated --replicas=1 --network botnet -p 8080:23 mirai-cnc:latest`
5. Start the bot service
	- `$ docker service create --name bots --mode replicated --replicas=2 --network botnet mirai-bot:latest`
6. Connect to the cnc
	- Telnet to the mapped port (`8080`)
	- Username: `root`
	- Password: `root`
	- Login credentials can be changed via the last line in  [db.sql](docker-cnc/db.sql)
	- Launch an attack (for more details refer to: [mirai](https://github.com/lejolly/mirai))
