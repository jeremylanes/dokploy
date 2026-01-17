# Dokploy Deployment

A containerized deployment of [Dokploy](https://dokploy.com/), an open-source alternative to Heroku, Vercel, and Netlify. This setup is designed to run behind **Traefik** as a reverse proxy.

## 📋 Prerequisites

Before starting this project, you **must** have a running instance of Traefik. 

The configuration in this repository depends on the `traefik_proxy` external network. We recommend using the following Traefik setup:
- **GitHub Repository**: [jeremylanes/traefik](https://github.com/jeremylanes/traefik)

Ensure Traefik is up and running before attempting to launch Dokploy.

## 🛠️ Installation & Setup

### 1. Clone the repository
```bash
git clone https://github.com/jeremylanes/dokploy
cd dokploy
```

### 2. Configure Environment Variables
Create a `.env` file in the root directory based on the following template:

```env
# Postgres Database Configuration
DOKPLOY_POSTGRES_DATABASE_NAME=dokploy
DOKPLOY_POSTGRES_USER=lane
DOKPLOY_POSTGRES_PASSWORD=alpine

# Domain Configuration
# This domain will be used by Traefik to route traffic to Dokploy
DOKPLOY_DOMAIN=tune.local
```

### 3. Launch the Services
This project includes a set of scripts in the `bin/` directory to simplify management.

To start Dokploy:
```bash
./bin/up
```

## 🚀 Usage & Commands

The following helper scripts are available in the `bin/` directory:

| Script | Description |
| :--- | :--- |
| `./bin/up` | Builds and starts all services in detached mode. |
| `./bin/down` | Stops and removes all project containers. |
| `./bin/logs` | Displays real-time logs for all services. |

## 🌐 Network Architecture

This project uses two Docker networks:
1.  **`dokploy_network`**: An internal network for communication between Dokploy, Postgres, and Redis.
2.  **`traefik_proxy`**: An external network used to expose the Dokploy UI through Traefik.

## 🔒 Security Note

-   **Docker Socket**: ⚠️ The mounting of `/var/run/docker.sock` gives the Dokploy container full control over the host's Docker daemon. This is required for Dokploy to manage other containers but represents a significant security risk if the application is compromised.
-   **Database**: The Postgres database is not exposed to the host; it is only accessible via the internal network.
-   **HTTPS**: Traefik handles SSL termination automatically. Ensure your `DOKPLOY_DOMAIN` points to your server's IP.

---
*Created with ❤️ for professional deployments.*
