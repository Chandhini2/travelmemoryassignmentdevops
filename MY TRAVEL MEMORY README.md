Travel Memory Application Deployment

Below is the Quick Reference Summary about the project resource used and its purpose

| Component        | Purpose                                      | Technology Used |
|------------------|----------------------------------------------|-----------------|
| EC2 Instance     | Hosts the MERN application                   | AWS EC2         |
| Nginx            | Acts as a reverse proxy server               | Nginx           |
| PM2              | Manages and monitors Node.js processes       | PM2             |
| Cloudflare       | Provides domain management and SSL security  | Cloudflare      |
| Load Balancer    | Ensures scalability and high availability    | AWS ALB         |
| MongoDB Atlas    | Cloud-based database service                 | MongoDB Atlas   |

Objective & Project Overview:

To Create a full stack web application with High scalability ,availability and Security


# TravelMemory Deployment Architecture

```mermaid
flowchart TD

    U[User] --> B[Browser]

    B --> C[Cloudflare + SSL/TLS]

    C --> LB[AWS Application Load Balancer]

    subgraph AWS_VPC["AWS VPC (ap-southeast-2)"]

        LB --> EC1
        LB --> EC2

        subgraph AZ1["Availability Zone - ap-southeast-2a"]
            EC1[EC2 Instance - 1]

            N1[Nginx Reverse Proxy]
            F1[React Frontend]
            BE1[Node.js Backend]

            EC1 --> N1
            N1 --> F1
            F1 --> BE1
        end

        subgraph AZ2["Availability Zone - ap-southeast-2b"]
            EC2[EC2 Instance - 2]

            N2[Nginx Reverse Proxy]
            F2[React Frontend]
            BE2[Node.js Backend]

            EC2 --> N2
            N2 --> F2
            F2 --> BE2
        end

    end

    BE1 --> DB[(MongoDB Atlas)]
    BE2 --> DB
'''

## Features

- User-friendly travel memory interface
- MERN stack architecture
- Reverse proxy using NGINX
- SSL/TLS security with Cloudflare
- High availability using AWS Load Balancer
- Multi-instance deployment
- MongoDB Atlas cloud database
- Process management using PM2

## Tech Stack

### Frontend
- React.js

### Backend
- Node.js
- Express.js

### Database
- MongoDB Atlas

### DevOps / Cloud
- AWS EC2
- AWS Application Load Balancer
- NGINX
- PM2
- Cloudflare

## Deployment Workflow

1. Created EC2 instances in AWS.
2. Installed Node.js and required dependencies.
3. Cloned the TravelMemory repository into EC2.
4. Configured backend and frontend services.
5. Installed and configured NGINX as reverse proxy.
6. Used PM2 to manage Node.js application processes.
7. Configured MongoDB Atlas for cloud database connectivity.
8. Created AWS Application Load Balancer.
9. Added EC2 instances to target group.
10. Configured Cloudflare DNS and SSL/TLS.
11. Verified load balancing and application accessibility.

## Load Balancing

AWS Application Load Balancer distributes incoming traffic across multiple EC2 instances to improve scalability, availability, and fault tolerance.

## Reverse Proxy Configuration

NGINX was configured as a reverse proxy server to route frontend and backend requests efficiently.

## Process Management

PM2 was used to run and monitor Node.js backend services continuously and restart applications automatically during failures.

## SSL & Security

Cloudflare was configured to provide:
- DNS management
- SSL/TLS encryption
- Secure HTTPS access
- Performance optimization

## High Availability

The application was deployed across multiple EC2 instances in different availability zones to ensure redundancy and minimize downtime.

## Challenges Faced and Resolutions

### 1. Domain Showing Infinite Loading

While accessing the application using the domain name, the website displayed only a loading screen, whereas it worked correctly using the EC2 public IP address. Since EC2 public IPs are dynamic and may change after instance restart, the NGINX upstream configuration was modified by replacing the EC2 public IPs with `localhost`.

### Updated NGINX Configuration

Path: `/etc/nginx/sites-available/default`

```nginx
# Upstream backend servers
upstream backend_servers {

    # EC2 Public IPs (optional for load balancing)
    # server 54.252.182.210;
    # server 3.27.228.142;

    # Local backend server
    server localhost:3000;
}
```

---

### 2. Duplicate DNS Record Issue

Encountered the following Cloudflare DNS error:

```text
An A, AAAA, or CNAME record with that host already exists.
```

This issue was resolved by identifying and removing duplicate DNS A records from Cloudflare.

---

### 3. Invalid Nameserver Configuration

The domain initially showed an invalid configuration issue. This was resolved by updating the Cloudflare-provided nameservers in the Namecheap domain management portal.

### Steps Followed

1. Navigated to the Namecheap Domain List.
2. Selected the **Manage** option for the domain.
3. Updated the Cloudflare nameservers under the **Custom DNS** section.
4. Waited a few minutes for DNS propagation until the Cloudflare status became active.

---

### 4. Web Server Down Error

The “Web Server Down” issue was resolved by:
- Restarting the EC2 instance
- Verifying backend services
- Clearing browser cache after DNS propagation

---

### 5. Final Cloudflare DNS Configuration

Configured the required DNS records successfully in Cloudflare to enable secure domain access and proper load balancing functionality.

## SCREENSHOTS

PFA travelmemory.docx

## Future Improvements

- Implement CI/CD pipeline
- Add Docker containerization
- Configure Auto Scaling Group
- Add monitoring and logging
- Improve security hardening

## Author

Developed and deployed by Chandhini Shri V.S


