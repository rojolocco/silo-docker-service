# MinIO Service Documentation

This document provides details about the MinIO service configured in the Docker setup.

## Description

MinIO is a high-performance, S3-compatible object storage service that is designed for large-scale data storage.

## Services

- **MinIO**: The main service that runs the MinIO object storage.
  - **Container Name**: s3
  - **Image**: minio/minio:latest
  - **Ports**: Exposed on ports (9000, 9001)
  - **Volumes**: Data is persisted in `minio_data` volume, mapped to `/data` and configuration is stored in `minio_config` volume, mapped to `/root/.minio`
  - **Environment Variables**: Loaded from `.env`
  - **Command**: `server /data --console-address ":9001"`
  - **Restart Policy**: Always restart the container
  - **Resource Limits**: Memory limit of `512M` and CPU limit of `0.5`

## Installation

1. Ensure Docker is installed on your machine.
2. Clone the repository if you haven't already:

   ```bash
   git clone https://github.com/yourusername/your-repo.git
   ```

3. Navigate to the project directory:

   ```bash
   cd your-repo
   ```

4. Start the services using Docker Compose:

   ```bash
   docker-compose up -d
   ```

## Usage

- Access MinIO at [http://localhost:9000](http://localhost:9000) to manage your object storage.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
