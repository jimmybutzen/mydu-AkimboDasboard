
# Akimbo Admin Setup

This repository contains the Docker setup for the Akimbo Admin service. This application is designed to run within a Docker stack that connects with multiple services.

## Prerequisites

- Docker and Docker Compose installed on your machine.
- A Running server installation of mydu.

## Setting Up the Akimbo Admin

### Step 1: Update Docker Compose File

Add the following content to the server installation `docker-compose.yml` file:


```yaml
akimbo-admin:
  image: jimboakimbo/mydu-akimboadmin:latest
  ports:
    - '3010:3000'
  environment:
    NODE_ENV: production
    PG_USER: "postgres"
    PG_PASSWORD: "postgres"
    PG_HOST: "10.5.0.9"  # The static IP address of the postgres container
    PG_PORT: "5432"
    PG_DATABASE: "dual"  # Replace with your actual database name
    ADMIN_EMAIL: "EDIT THIS"
    ADMIN_PASSWORD: "EDIT THIS"
  command: npm start
  volumes:
    - ./akimboadmin/db:/app/prisma 
  networks:
    vpcbr:
      ipv4_address: 10.5.0.212
```
REPLACE "EDIT THIS" WITH EMAIL AND PASSWORD !!
> **Note:** Ensure that the indentation matches the existing services.
### Step 2: Run the Docker Compose

To start the Akimbo Admin service, navigate to the directory containing your `docker-compose.yml` file and run:

```bash
docker-compose up -d akimbo-admin
```

This will start the Akimbo Admin service on port 3010 of your host machine.

### Step 3: Initial Setup

On first-time use, you need to log into the Docker container and create an admin user. Run the following commands:

1. Find the container ID for the Akimbo service:

   ```bash
   docker ps
   ```

2. Use the container ID to log into the container:

   ```bash
   docker exec -it <Container_ID> sh
   ```

3. Inside the container, run the following command to create the admin user (this will set the ADMIN_EMAIL and ADMIN_PASSWORD you have changed in docker-compose):

   ```bash
   node create-admin.ts
   ```

### Step 4: Access the Admin Interface

Once the service is running, you can access the Akimbo Admin interface by navigating to `http://localhost:3010` in your web browser.

## Additional Notes

- The IP address for the PostgreSQL container (`PG_HOST`) and the admin credentials can be adjusted as needed.
- Make sure the PostgreSQL service is running and accessible from within the same Docker network.
- This version is intended for localhost use only , i am not responsible for a security breach if you open this to the public internet. 

![Project Logo](https://github.com/jimmybutzen/mydu-AkimboDasboard/blob/main/images/dashboard.PNG)
