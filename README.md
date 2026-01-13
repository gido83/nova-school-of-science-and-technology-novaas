# NOVA Asset Administration Shell (NOVAAS)

NOVA Asset Administration Shell (NOVAAS) is an open-source reference implementation and execution environment for the Asset Administration Shell (AAS) concept, developed by NOVA School of Science and Technology. It implements the AAS concept proposed by the Reference Architectural Model for Industrie 4.0 (RAMI4.0).

## Overview

The AAS implements the concept of the Digital Twin of a physical object, transforming it into an Industrie 4.0 component that facilitates interoperability in industrial applications.

## Getting Started

### Prerequisites

- Docker
- Docker Compose (for docker-compose setup)
- Git (for cloning the repository)

### Installation

This repository contains all necessary files to build a Docker image for NOVAAS. The repository comes preloaded with example files (documentation, assets, manifest) that demonstrate the component's functionality.

#### Using Dockerfile

1. Clone the repository:
   ```bash
   git clone https://gitlab.com/gidouninova/novaas.git
   cd novaas
   ```

2. Build the Docker image:
   ```bash
   docker build --build-arg AAS_VERSION=V3 -t novaas:latest .
   ```
   
   > **Note:** By default, NOVAAS provides backward compatibility with AAS V2 metamodel. To use V3, set the `AAS_VERSION` build argument to `V3` as shown above.

3. Run the container:
   ```bash
   docker run -d \
     --env PORT_FORWARDING=1872 \
     --env HOST=localhost \
     --env BROKER_SERVICE_HOST=localhost \
     --env BROKER_SERVICE_PORT=1883 \
     --env REPO_LOCATION=https://gitlab.com/novaas/catalog/nova-school-of-science-and-technology/novaas \
     -p 1872:1880 \
     novaas:latest
   ```

4. Access the NOVAAS dashboard at:
   ```
   http://localhost:1872/dashboard
   ```

   ![NOVAAS Main Screen](/source/images/novaas.jpg)

## Using dockerfile
The docker command to create the NOVAAS image is the following:

`dokcer build --build-arg AAS_VERSION=V3 -t name_of_the_image:ver .`

Once the image has been created the followig docker command can be used to start a new container that runs the NOVAAS image:

`docker run -d --env PORT_FORWARDING=1872 --env HOST=localhost --env BROKER_SERVICE_HOST=localhost --env BROKER_SERVICE_PORT=1883 --env REPO_LOCATION=https://gitlab.com/novaas/catalog/nova-school-of-science-and-technology/novaas -p 1872:1880 name_of_the_image:ver`

After executing the above commands, the NOVAAS will be accessible at the following link:

http://localhost:1872/dashboard

![Semantic description of image](/source/images/novaas.jpg)"NOVAAS Main Screen"

## Configuration

### Environment Variables

- `HOST`: The IP address of the host machine where NOVAAS is deployed. 
  - **Important**: Using `localhost` will only expose the dashboard within the UI on the host machine.
  
- `BROKER_SERVICE_HOST`: Hostname or IP address of the MQTT broker.
- `BROKER_SERVICE_PORT`: Port number for the MQTT broker (default: 1883).
- `PORT_FORWARDING`: External port for accessing the NOVAAS interface (default: 1872).
- `REPO_LOCATION`: URL to the NOVAAS catalog repository.

### MQTT Configuration

NOVAAS includes an embedded MQTT client for data publishing. You can configure it either:

1. Through the web interface at `http://localhost:1872`
2. Or by setting the environment variables mentioned above

### Backend Access

Access the NOVAAS backend at:
```
http://localhost:1872
```

Default login credentials:
- **Username**: admin
- **Password**: password

> **Security Note**: Change these default credentials in production environments.

![NOVAAS Backend](/source/images/aasAssetConnection.png)



## Using Docker Compose

### Quick Start

1. Clone the repository (if not already done):
   ```bash
   git clone https://gitlab.com/gidouninova/novaas.git
   cd novaas
   ```

2. Start NOVAAS:
   ```bash
   docker-compose up -d
   ```

### Building the Image

To rebuild the Docker image:

```bash
docker-compose build
```

### Configuration

Customize the following environment variables in the `.env` file:

- `PORT_FORWARDING`: External port (default: 1872)
- `HOST`: Host IP address
- Other environment variables as needed

### Stopping NOVAAS

```bash
docker-compose down
```

## Using Pre-built Images

Pre-built Docker images are available in the GitLab Container Registry:

[View NOVAAS Container Registry](https://gitlab.com/novaas/catalog/nova-school-of-science-and-technology/novaas/container_registry)

To pull the latest image:

```bash
docker pull registry.gitlab.com/novaas/catalog/nova-school-of-science-and-technology/novaas:latest
```

## Customizing NOVAAS

NOVAAS is designed to be highly customizable. To create your own version:

### 1. Environment Model

1. Place your environment model file in the `files/` directory
2. **Important**: The file must be named `model.aasx`
3. The model should follow the AAS data model specification:
   - [Details of the Asset Administration Shell - Part 1](https://www.plattform-i40.de/PI40/Redaktion/EN/Downloads/Publikation/Details_of_the_Asset_Administration_Shell_Part1_V3.html)
   - Created using the [AASX Package Explorer](https://github.com/eclipse-aaspe/package-explorer)
   - **Format**: Must be saved as "aasx w/ JSON"

### 2. Authentication

Update the `files/httpauth` file with your authentication settings.

### 3. Asset Connection

Modify the connection logic in the "DPDM/OperationalData" flow to connect NOVAAS to your asset.

### Implementation Notes

#### AASX Model
- The provided `model.aasx` was created using AASX Explorer with the "aasx w/ JSON" option

#### Asset Connection

To connect your physical asset to the digital twin:

1. Navigate to the "DPDM/OperationalData" flow
2. Replicate the existing submodel element pattern for new elements
3. The connection between the model and real data is based on identifiers

![Submodel Element Pattern](/source/images/Screenshot%202021-12-05%20at%2014.03.26.png)
*Submodel element pattern example*

![Submodel Element Properties](/source/images/Screenshot%202021-12-08%20at%2021.18.png)
*Submodel element property configuration*

## Supported AAS Features

### Submodel Elements

NOVAAS currently supports the following AAS V3 Submodel Elements:

- `SubmodelElementCollection`
- `Property`
- `MultiLanguageProperty`
- `File`
- `Blob`
- `Operation`
- `BasicElementEvent`

> **Note**: Support for additional Submodel Elements will be added in future updates.


## Demo & Tutorial

[![NOVAAS Demo Video](https://img.youtube.com/vi/VIDEO_ID/0.jpg)](https://gitlab.com/gidouninova/novaas/-/raw/master/source/videos/NOVAAS_myMovie.mp4)

[Watch the full tutorial video](https://gitlab.com/gidouninova/novaas/-/raw/master/source/videos/NOVAAS_myMovie.mp4)

## License

[Specify License Here]

## Contributing

Contributions are welcome! Please see our [contributing guidelines](CONTRIBUTING.md) for details.

## Support

For support, please open an issue in the [issue tracker](https://gitlab.com/gidouninova/novaas/-/issues).
