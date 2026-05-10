
# Portainer Ubuntu

Use this minimal reusable install script for Portainer only.

Use this minimal reusable install script for Portainer only.

```bash id="ol7m7f"
#!/bin/bash

set -e

echo "=================================="
echo " Starting Portainer Setup"
echo "=================================="

# Check Docker
if ! command -v docker &> /dev/null
then
    echo "Docker is not installed."
    exit 1
fi

# Create volume if not exists
docker volume create portainer_data >/dev/null

# Remove old container if exists
docker rm -f portainer 2>/dev/null || true

# Pull latest image
docker pull portainer/portainer-ce:latest

# Start Portainer
docker run -d \
  --name portainer \
  --restart unless-stopped \
  -p 8000:8000 \
  -p 9443:9443 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:latest

SERVER_IP=$(hostname -I | awk '{print $1}')

echo ""
echo "=================================="
echo " Portainer Started Successfully"
echo "=================================="
echo ""
echo "URL:"
echo "https://$SERVER_IP:9443"
echo ""

docker ps | grep portainer
```

Save:

```bash id="jlzxtv"
nano install-portainer.sh
```

Make executable:

```bash id="h3ucwf"
chmod +x install-portainer.sh
```

Run:

```bash id="7h9h57"
./install-portainer.sh
```
