# Minio for s3 Self Hosted Bucket

### Installing Docker
The error you’re encountering is caused by a malformed entry in the `/etc/apt/sources.list.d/docker.list` file, which is preventing your system from reading the Docker repository.

Let's fix it step by step:

### 1. **Remove the Existing Malformed Docker Source**

To clear up the issue, let's remove the existing `docker.list` file:

```bash
sudo rm /etc/apt/sources.list.d/docker.list
```

### 2. **Re-add the Docker Repository Correctly**

Now, let's re-add the Docker repository in a clean manner.

#### a. **Update your system and install prerequisites**:

```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
```

#### b. **Add Docker's GPG key**:

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

#### c. **Re-add the Docker repository**:

Use this corrected command to add the Docker repository. This will prevent the malformed entry issue:

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 3. **Update Package List and Install Docker**

Now that the repository is correctly added, update your package list again:

```bash
sudo apt-get update
```

Then, install Docker:

```bash
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
```

### 4. **Install Docker Compose Plugin (if needed)**

To install the Docker Compose plugin, run:

```bash
sudo apt-get install -y docker-compose-plugin
```

### 5. **Verify Installation**

Finally, check that Docker is installed properly:

```bash
docker --version
docker compose version
```

This should resolve the issue with the malformed entry and allow you to install Docker and Docker Compose.

## Docker Compose file
- Create one docker-compose.yml file
```yml
services:
  s3:
    image: minio/minio:latest
    ports:
      - "9000:9000"
      - "9099:9099"
    environment:
      MINIO_ROOT_USER: xxxx
      MINIO_ROOT_PASSWORD: xxxxx
    volumes: 
      - /storage_block/backup-supbase:/data
    command: server --address "0.0.0.0:9099" --console-address "0.0.0.0:9000" /data
    restart: always
```

## Cloudflare tunnel
- Create the cloudflare zero trust activate it, 
- Click on networks>tunnel
- Create a tunnel name
- Configure the tunnel
- Run the docker installation on the cloud server, which have minio installed
- Add the extra code to keep it in background and restard when crashed
```
-d --name cloudflaretunnel --restart unless-stopped
```
- Full code will look like
```
sudo docker run -d --name cloudflaretunnel --restart unless-stopped cloudflare/cloudflared:latest tunnel --no-autoupdate run --token xxxxxxxx
```
- Then you will see the tunnel conection in cloudflare dashboard
- Then add the public hostnames- for minio console
```
subdomain: s3
domain: yourdomain.com
type: http (cloudflare will make it https)
url: yourcloudserverip:9000
```
- Add another public hostnames- for minio api, for supabase backup
```
subdomain: minio
domain: yourdomain.com
type: http (cloudflare will make it https)
url: yourcloudserverip:9099 (same as the docker config, if given 9091, then use that)
```

## Note
- If you are running on oracel server, don't forget to add the ingress security rules to open port `9000` and `9099` as given in the config files

## Creating bucket, users, api in minio console
- Create the user with `write only permission`
- Create the api, secret key with permission
- create the bucket and update the bucket name in the coolify, supabase backup settings
