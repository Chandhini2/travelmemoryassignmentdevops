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

ARCHITECTURE DIAGRAM:
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




