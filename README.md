# Docker-WordPress-Installation-Guide
Step-by-Step Docker WordPress Installation Guide
## Introduction
This guide will walk you through setting up WordPress using Docker and Docker Compose. Whether you're a beginner or have some experience, these step-by-step instructions will help you get WordPress running in a Docker container.

## Prerequisites
- Ubuntu Linux 
- Basic command line knowledge
- VirtualBox installed (if using a virtual machine)
- Internet connection

## Step 1: Installing Docker
First, let's get Docker installed on your system.

```
# Update your system
sudo apt update
sudo apt upgrade -y

# Install required packages
sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg
```

💡 **Tip**: If you get any "Unable to locate package" errors, run `sudo apt update` again.

Now install Docker:
```
# Install Docker
sudo apt install docker.io containerd

# Start and enable Docker
sudo systemctl start docker
sudo systemctl enable docker

# Verify installation
docker --version
```

## Step 2: Installing Docker Compose
Docker Compose makes it easy to manage multi-container applications.

```
# Install Docker Compose
sudo apt install docker-compose-plugin

# Verify installation
docker compose version
```

## Step 3: Setting Up WordPress
Let's create a directory for our WordPress project and set up the necessary files.

```
# Create and enter project directory
mkdir ~/wordpress
cd ~/wordpress

# Create docker-compose file
nano docker-compose.yml
```

Copy this configuration into docker-compose.yml:
```yaml
version: '3'

services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress    # Change this!
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress             # Change this!

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8080:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress      # Match with MYSQL_PASSWORD
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wordpress_data:/var/www/html

volumes:
  db_data: {}
  wordpress_data: {}
```

⚠️ **Security Note**: For production use, change the default passwords!

## Step 4: Start the Containers
```
# Start WordPress and MySQL containers
docker compose up -d

# Verify containers are running
docker ps
```

## Step 5: Configure Port Forwarding (VirtualBox Users Only)
If you're using VirtualBox, you'll need to set up port forwarding:

1. Stop your VM
2. In VirtualBox Manager:
   - Select your VM → Settings → Network
   - Adapter 1 → Advanced → Port Forwarding
   - Add new rule:
     * Name: WordPress
     * Protocol: TCP
     * Host Port: 8080
     * Guest Port: 8080
3. Start your VM

## Step 6: Access WordPress
Open your web browser and go to:
- `http://localhost:8080` (or `http://your-server-ip:8080`)

Follow the WordPress setup:
1. Choose your language
2. Set up your site:
   - Site Title: Your choice
   - Username: Create a secure admin username (avoid "admin")
   - Password: Use a strong password
   - Email: Your email address

## Useful Commands

### Check Container Status
```
# View running containers
docker ps

# Check container logs
docker compose logs

# View WordPress logs specifically
docker compose logs wordpress
```

### Start/Stop Containers
```
# Stop containers
docker compose down

# Start containers
docker compose up -d

# Restart containers
docker compose restart
```

## Troubleshooting Guide

### Problem: Can't access WordPress
1. Check if containers are running:
   ```
   docker ps
   ```
2. Check logs:
   ```
   docker compose logs wordpress
   ```
3. Verify port forwarding (VirtualBox users)

### Problem: Database connection errors
1. Verify environment variables in docker-compose.yml
2. Check database logs:
   ```
   docker compose logs db
   ```
3. Restart containers:
   ```
   docker compose restart
   ```

## Tips for Success
1. Always change default passwords
2. Regularly backup your data
3. Keep Docker and WordPress updated
4. Monitor container logs for issues

## Need Help?
- Check container logs: `docker compose logs`
- Verify container status: `docker ps`
- Review configuration: `cat docker-compose.yml`

## What's Next?
After successful installation, you can:
1. Install WordPress themes
2. Add plugins
3. Create your first post
4. Configure permalinks
5. Set up backups
  
