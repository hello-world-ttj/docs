## Installation

### Installing Docker

To install Docker on your system, run the following command:

### Install Docker on EC2

- Update the package index:

```bash
sudo yum update -y
```

- Install Docker:

```bash
sudo yum install -y docker
```

- Start Docker and enable it to start on boot:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

- Add your user to the docker group to run Docker commands without sudo:

```bash
sudo usermod -aG docker ec2-user
```

### Install MongoDB on EC2

- Create a MongoDB repository file:

```bash
echo "[mongodb-org-5.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/amazon/2/mongodb-org/5.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-5.0.asc" | sudo tee /etc/yum.repos.d/mongodb-org-5.0.repo
```

- Install MongoDB:

```bash
sudo yum install -y mongodb-org
```

- Start MongoDB and enable it to start on boot:

```bash
sudo systemctl start mongod
sudo systemctl enable mongod
```

### Install Nginx on EC2

- Install Nginx:

```bash
sudo yum install -y nginx
```

- Start Nginx and enable it to start on boot:

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

### Configure Nginx as a Reverse Proxy

- Open the Nginx configuration file:

```bash
sudo nano /etc/nginx/nginx.conf
```

- Updating server_name with your domain:

```bash
server {
        listen       80;
        server_name  yourdomain.com www.yourdomain.com;

        location / {
            proxy_pass http://localhost:3000;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
```

- Test the Nginx configuration:

```bash
sudo nginx -t
```

- Reload Nginx to apply the changes:

```bash
sudo systemctl reload nginx
```

### Install Certbot on EC2

- Install Certbot:

```bash
sudo yum install -y certbot python3-certbot-nginx
```

- Obtain SSL certificates using Certbot:

```bash
sudo certbot
```
