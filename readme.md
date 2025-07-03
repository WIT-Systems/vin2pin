# Web Adaptation: Smartra VIN to PIN Converter

This project is a web-based adaptation of the [Smartra VIN to PIN Converter](https://github.com/Dante383/smartra-vin-to-pin) originally developed by @Dante383. It provides a simple, accessible interface for converting Vehicle Identification Numbers (VINs) to immobilizer PINs directly within a web browser.

# How to Use

You have several ways to access and use this application:


## Option 1: Access Online (Recommended for Quick Use)

The application is hosted and publicly available for direct access:

- Visit: [https://vin2pin.wicki.sbs/](https://vin2pin.wicki.sbs/)


## Option 2: Directly Opening the HTML File

This is the simplest method if you prefer to run it locally without a web server.

1. **Download the Project**  
   Obtain all the project files (e.g., by cloning the repository or downloading the ZIP file).

2. **Open `index.html`**  
   Locate the `index.html` file in the downloaded project directory.

3. **Launch in Browser**  
   Double-click `index.html` or drag it into your web browser. The application will open directly in your browser.


## Option 3: Running with Docker (Recommended for Local Development/Workshops)

For a consistent environment, you can run the application as a Docker container.

### Prerequisites

- Docker installed on your system.

### Steps

1. **Pull the Docker Image**  
   The image is available on GitHub Container Registry (GHCR):
   ```bash
   docker pull ghcr.io/wit-systems/vin2pin:sha-7340459
   ```
2. **Run the Docker Container**
   ```bash
   docker run -p 8080:80 ghcr.io/wit-systems/vin2pin:sha-7340459
   ```
3. **Access the Application**
    Open your web browser and navigate to:

    http://localhost:8080

    You should now see the Smartra VIN to PIN Converter running.

## Option 4: Docker Compose with Traefik (Advanced Users)

For advanced users managing multiple services, you can integrate **vin2pin** with [Traefik](https://traefik.io/) for reverse proxying and automatic SSL.

---

### Prerequisites

- Docker and Docker Compose installed.
- An existing Traefik setup with an external Docker network (e.g., `traefik-net`).

---

### `docker-compose.yml` Example

Create a file named `docker-compose.yml` with the following content:

    ```yaml
    services:
        vin2pin:
            image: ghcr.io/wit-systems/vin2pin:sha-7340459
            restart: unless-stopped
            labels:
             - traefik.enable=true
             - traefik.http.routers.vin2pin.rule=Host(`YOUR.HOSTNAME.EXAMPLE`)
             - traefik.http.routers.vin2pin.entrypoints=websecure, web
             - traefik.http.services.vin2pin.loadbalancer.server.port=80
             - traefik.docker.network=traefik-net
            networks:
             - traefik-net

    networks:
        traefik-net:
            external: true
    ```

### Important Notes
Replace YOUR.HOSTNAME.EXAMPLE with your actual domain name.

Ensure your traefik-net network is already defined and managed externally in your Traefik setup.

### Steps to Run

Save the file as docker-compose.yml.

Start the container using Docker Compose:

    docker compose up -d

Access the application via your configured domain:

    https://YOUR.HOSTNAME.EXAMPLE

If everything is configured correctly, the Smartra VIN to PIN Converter should be accessible through your domain with HTTPS.
