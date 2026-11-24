# 🍽️ Ember & Olive – Restaurant Website Deployment Using Docker & Nginx on AWS EC2

A modern, responsive restaurant landing page deployed using **Nginx inside a Docker container** on an **AWS EC2 instance running Amazon Linux**.

This project demonstrates Docker containerization, custom Docker image creation, Nginx web server configuration, and cloud deployment using AWS EC2.

---

## 📌 Project Overview

**Ember & Olive** is a modern Mediterranean restaurant landing page featuring an elegant design, interactive menu, customer reviews, and a reservation interface.

The website is containerized using Docker and served through the Nginx web server on an AWS EC2 instance.

### Project Details

| Property | Description |
|---|---|
| Project Name | Ember & Olive |
| Cloud Provider | AWS |
| Cloud Service | EC2 |
| Operating System | Amazon Linux |
| Containerization | Docker |
| Web Server | Nginx |
| Frontend | HTML, CSS, JavaScript |
| Deployment Port | 5000 |
| Container Port | 80 |

---

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| HTML5 | Website structure |
| CSS3 | Styling and responsive design |
| JavaScript | Website interactivity |
| Docker | Application containerization |
| Nginx | Web server |
| AWS EC2 | Cloud hosting |
| Amazon Linux | Server operating system |
| Git | Version control |
| GitHub | Source code management |

---

## 🏗️ Deployment Architecture

The application follows this deployment architecture:

```text
                  INTERNET
                     |
                     |
                PUBLIC IP
                     |
                     |
              AWS EC2 INSTANCE
                Amazon Linux
                     |
                     |
                DOCKER ENGINE
                     |
                     |
              DOCKER CONTAINER
                     |
                     |
                   NGINX
                     |
                     |
          /usr/share/nginx/html/
                     |
                     |
                 index.html
                     |
                     |
             CUSTOM WEBSITE
```

### Request Flow

1. A user accesses the EC2 public IP through a web browser.
2. AWS Security Group allows incoming traffic on port 5000.
3. Docker receives the request through the published host port.
4. Docker forwards the request to port 80 inside the container.
5. Nginx processes the HTTP request.
6. Nginx serves the custom `index.html` webpage.

---

## 📂 Project Structure

```text
nginx-docker-deployment/
│
├── index.html
│
├── Dockerfile
│
└── README.md
```

### File Description

| File | Description |
|---|---|
| `index.html` | Main restaurant landing page |
| `Dockerfile` | Instructions to build the custom Nginx image |
| `README.md` | Project documentation |

---

# 🚀 Deployment Steps

## Step 1: Launch an AWS EC2 Instance

1. Log in to AWS Management Console.
2. Navigate to EC2.
3. Launch a new EC2 instance.
4. Select Amazon Linux as the operating system.
5. Choose an appropriate instance type.
6. Configure the Security Group.
7. Launch the instance.
8. Connect to the instance using SSH.

## Step 2: Install Docker on Amazon Linux

Update the system packages:

```bash
yum update -y
```

Install Docker:

```bash
yum install docker -y
```

Start Docker:

```bash
systemctl start docker
```

Enable Docker to start automatically after reboot:

```bash
systemctl enable docker
```

Verify Docker installation:

```bash
docker --version
```

Check Docker service status:

```bash
systemctl status docker
```

## Step 3: Create Project Directory

Create a directory for the project:

```bash
mkdir nginx-project
```

Navigate into the directory:

```bash
cd nginx-project
```

Create the HTML file:

```bash
vim index.html
```

Add the restaurant landing page HTML, CSS, and JavaScript code.

Save the file:

- Press `Esc`.
- Type `:wq`.
- Press Enter.

## Step 4: Create Dockerfile

Create a file named `Dockerfile`:

```bash
vim Dockerfile
```

Add the following configuration:

```dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Dockerfile Explanation

**1. FROM nginx:latest**

Uses the official Nginx Docker image as the base image.

**2. COPY**

Copies the custom HTML file into the default Nginx web directory.

```text
/usr/share/nginx/html/index.html
```

**3. EXPOSE 80**

Documents that Nginx listens on port 80 inside the container.

**4. CMD**

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

Starts Nginx in the foreground so that the container continues running.

## Step 5: Build Docker Image

Build the custom Docker image:

```bash
docker build -t my-nginx:v1 .
```

### Command Explanation

| Option | Description |
|---|---|
| `docker build` | Builds a Docker image |
| `-t` | Assigns a name and tag |
| `my-nginx` | Image name |
| `v1` | Image version |
| `.` | Current directory as build context |

Verify the image:

```bash
docker images
```

Expected output:

```text
REPOSITORY    TAG
my-nginx      v1
```

## Step 6: Run Docker Container

Create and start the container:

```bash
docker run -d -p 5000:80 --name nginx-cont my-nginx:v1
```

### Command Explanation

| Option | Description |
|---|---|
| `docker run` | Creates and starts a container |
| `-d` | Runs the container in detached mode |
| `-p 5000:80` | Maps EC2 port 5000 to container port 80 |
| `--name nginx-cont` | Assigns a container name |
| `my-nginx:v1` | Specifies the Docker image |

### Understanding Port Mapping

```text
EC2 HOST PORT          CONTAINER PORT
     5000       --->        80
                             |
                             |
                           NGINX
```

When a user accesses port 5000 on the EC2 public IP, Docker forwards the request to port 80 inside the Nginx container.

## Step 7: Verify Container Status

Check running containers:

```bash
docker ps
```

Check all containers:

```bash
docker ps -a
```

Check container logs:

```bash
docker logs nginx-cont
```

Test the website locally:

```bash
curl http://localhost:5000
```

## Step 8: Configure AWS Security Group

To access the website externally, configure the EC2 Security Group.

Navigate to:

**AWS Console → EC2 → Instances → Select Instance → Security → Security Groups → Edit Inbound Rules**

Add the following inbound rules:

| Type | Protocol | Port | Source |
|---|---|---|---|
| SSH | TCP | 22 | My IP |
| Custom TCP | TCP | 5000 | 0.0.0.0/0 |

Save the rules.

**Note:** Restrict SSH access to your own IP. Port 5000 is publicly accessible in this example.

## Step 9: Access the Website

Copy the Public IPv4 address of your EC2 instance.

Open your browser:

```text
http://YOUR_EC2_PUBLIC_IP:5000
```

Your Ember & Olive restaurant landing page should now be accessible over the internet.

---

# 🐳 Useful Docker Commands

| Command | Description |
|---|---|
| `docker images` | Lists available Docker images |
| `docker ps` | Lists running containers |
| `docker ps -a` | Lists all containers |
| `docker build -t my-nginx:v1 .` | Builds Docker image |
| `docker run -d -p 5000:80 --name nginx-cont my-nginx:v1` | Runs a container |
| `docker stop nginx-cont` | Stops a container |
| `docker start nginx-cont` | Starts a stopped container |
| `docker restart nginx-cont` | Restarts a container |
| `docker logs nginx-cont` | Displays container logs |
| `docker inspect nginx-cont` | Displays container information |
| `docker rm -f nginx-cont` | Removes a container |
| `docker rmi my-nginx:v1` | Removes a Docker image |

---

# 🔄 Updating the Website

Whenever changes are made to `index.html`, rebuild the Docker image to include the updated file.

### 1. Modify HTML

Update the website content:

```bash
vim index.html
```

### 2. Build a New Image

```bash
docker build -t my-nginx:v2 .
```

### 3. Remove the Old Container

```bash
docker rm -f nginx-cont
```

### 4. Run a New Container

```bash
docker run -d -p 5000:80 --name nginx-cont my-nginx:v2
```

### 5. Verify

```bash
docker ps
```

Refresh your browser to see the updated website.

---

# 🐛 Troubleshooting

## 1. Container Exits Immediately

Check container logs:

```bash
docker logs nginx-cont
```

Make sure Nginx runs in the foreground:

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

A spelling mistake in `daemon` can cause Nginx to fail.

## 2. Port Already in Use

Check whether port 5000 is occupied:

```bash
ss -tulnp | grep :5000
```

If another application is using the port, choose a different host port.

## 3. Website Not Accessible

Check the following:

- EC2 instance is running.
- Docker service is active.
- Container is running.
- Port 5000 is allowed in the Security Group.
- Docker port mapping is correct.

Check container status:

```bash
docker ps -a
```

## 4. Changes Not Appearing on Website

Rebuild the Docker image after modifying the HTML file.

Also try refreshing the browser without using its cached content.

---

# 📚 Key Learnings

Through this project, I learned:

- Installing and configuring Docker on Amazon Linux.
- Understanding Docker images and containers.
- Writing a custom Dockerfile.
- Building custom Docker images.
- Running Nginx inside a Docker container.
- Understanding Docker port mapping.
- Deploying a website on AWS EC2.
- Configuring AWS Security Groups.
- Troubleshooting container startup failures.
- Managing Docker containers and images.
- Understanding the deployment workflow of a containerized application.

---

# 🔮 Future Improvements

- Push the custom Docker image to Docker Hub.
- Automate deployment using GitHub Actions.
- Configure a custom domain.
- Enable HTTPS using SSL/TLS certificates.
- Implement Docker volumes for easier website updates.
- Configure Nginx reverse proxy.
- Add monitoring and logging.
- Implement a CI/CD pipeline.

---

## 👨‍💻 Author

**Adarsh Sharma**



---

## ⭐ Acknowledgements

- Docker Official Documentation
- Nginx Official Documentation
- AWS EC2 Documentation

---

**If you found this project useful, consider giving this repository a ⭐ on GitHub!**
