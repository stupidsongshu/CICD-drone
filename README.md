# VuePress Blog
- GitHub + Drone

## CI/CD
### Drone
```sh
# command
node ./scripts/deploy.js staging001

openssl rand -hex 16
9a3e564e1d6d8649d4111f14332c0292

# 安装 docker server
docker run \
  --volume=/var/lib/drone:/data \
  --env=DRONE_AGENTS_ENABLED=true \
  --env=DRONE_GITHUB_SERVER=https://github.com \
  --env=DRONE_GITHUB_CLIENT_ID=355e068718b368c85388 \
  --env=DRONE_GITHUB_CLIENT_SECRET=7244892c28295ae9e9b7cb83913aa2ac8d2d5af7 \
  --env=DRONE_RPC_SECRET=9a3e564e1d6d8649d4111f14332c0292 \
  --env=DRONE_SERVER_HOST=111.229.81.101 \
  --env=DRONE_SERVER_PROTO=http \
  --publish=80:80 \
  --publish=443:443 \
  --restart=always \
  --detach=true \
  --name=drone \
  drone/drone:1

# 安装 drone agent
docker run -d \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -e DRONE_RPC_PROTO=http \
  -e DRONE_RPC_HOST=111.229.81.101 \
  -e DRONE_RPC_SECRET=9a3e564e1d6d8649d4111f14332c0292 \
  -e DRONE_RUNNER_CAPACITY=2 \
  -e DRONE_RUNNER_NAME=node \
  -p 3000:3000 \
  --restart always \
  --name runner \
  drone/drone-runner-docker:1
```
