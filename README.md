`mkdir cysic-docker && cd cysic-docker`

```shell
cat > build_and_run.sh << 'EOF'
#!/bin/bash

cat > Dockerfile << 'EOD'
from ubuntu:22.04

run apt-get update && apt-get install -y \
    curl \
    bash \
    ca-certificates \
    libssl-dev \
    libgmp-dev \
    libstdc++6 \
    wget \
 && rm -rf /var/lib/apt/lists/*

workdir /root

run curl -L https://github.com/cysic-labs/cysic-phase3/releases/download/v1.0.0/setup_linux.sh -o setup_linux.sh && \
    bash setup_linux.sh 0x-Fill-in-your-reward-address-here

workdir /root/cysic-verifier

cmd ["bash", "start.sh"]
EOD

docker build -t cysic-verifier .

docker run -d --restart unless-stopped --name cysic-verifier -v ~/.cysic:/root/.cysic cysic-verifier
EOF
```

`chmod +x build_and_run.sh`

`./build_and_run.sh`

`docker ps`

`docker logs -f cysic-verifier`

`docker stop cysic-verifier && docker rm -f cysic-verifier`

