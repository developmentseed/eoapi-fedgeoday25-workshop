# eoAPI FedGeoDay25 workshop

This repository contains the materials for the eoAPI workshop at FedGeoDay25 in Washington D.C. on April 23, 2025.

For the in-person workshop we deployed eoAPI using [eoapi-cdk](https://github.com/developmentseed/eoapi-cdk) constructs in AWS us-west-2.
Participants in the in-person workshop were provided with credentials for the `pgstac` database so they could interact with it during the tutorials.

Now the resources in AWS have been removed but we have updated this repository to work entirely locally in a docker network.

## Running the tutorial

This project uses a docker compose file to spin up a full eoAPI stack and a Jupyter Hub server that you can use to interact with the eoAPI services via Jupyter notebooks.

## Install docker and docker compose

- **Windows/Mac**: Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- **Linux**: Follow the [official installation instructions](https://docs.docker.com/engine/install/) for your distribution

Docker Compose is included with Docker Desktop for Windows and Mac. For Linux, follow the [Docker Compose installation guide](https://docs.docker.com/compose/install/).

### Get authenticated with the GitHub Container Registry

If you have already logged into into `ghcr` via docker then you can skip this step.

#### Create a Personal Access Token (PAT) on GitHub

- Go to your GitHub account settings: <https://github.com/settings/tokens>
- Click "Generate new token" (classic)
- Give your token a descriptive name (e.g., "Docker GHCR Access")
- Set an expiration date (or choose "No expiration" if appropriate)
- Select the following scopes:
  - `read:packages` (to download container images)
  - Also select `write:packages` if you plan to push images
- Click "Generate token"
- **Important**: Copy the token immediately as you won't be able to see it again

#### Log in to GitHub Container Registry using Docker

**Windows**:

```
docker login ghcr.io -u YOUR_GITHUB_USERNAME -p YOUR_GITHUB_PAT
```

**macOS/Linux**:

```bash
echo YOUR_GITHUB_PAT | docker login ghcr.io -u YOUR_GITHUB_USERNAME --password-stdin
```

Or alternatively:

```bash
docker login ghcr.io -u YOUR_GITHUB_USERNAME -p YOUR_GITHUB_PAT
```

Note: Replace `YOUR_GITHUB_USERNAME` with your actual GitHub username and `YOUR_GITHUB_PAT` with the token you created.

If successful, you should see a "Login Succeeded" message.

Once authenticated, Docker will be able to pull the required container images from GitHub Container Registry when you run `docker compose up`.

### Clone this repository and start the docker network

```bash
git clone https://github.com/developmentseed/eoapi-fedgeoday25-workshop.git
cd eoapi-fedgeoday25-workshop
docker compose up
```

This will start up 6 services:

- pgstac: postgres database with pgstac installed, running on port 5439
- stac-fastapi-pgstac: STAC API available on port 8081
- titiler-pgstac: dynamic tiler available on port 8082
- tipg: vector feature/tile server available on port 8083
- stac-browser: beautiful interface for browsing a STAC API available on port 8085
- Jupyter Hub: interactive compute environment where you can browse the tutorial materials interactively, available on port 8888

4. Open the Jupyter Hub in your web browser at `http://localhost:8888` and go through the tutorials in the `/docs` folder!

## Deploying to AWS

If you are interested deplying a production-ready version of the eoAPI stack, you can deploy the same stack that we used in the in-person workshop to AWS using eoapi-cdk constructs. See [DEPLOYMENT.md](./DEPLOYMENT.md) for details.
