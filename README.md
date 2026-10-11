# Install Docker Compose

This GitHub Action installs Docker Compose v2

## Usage

```yaml
jobs:
  my-awesome-job:
    steps:
      - name: Install Compose
        uses: ndeloof/install-docker-compose
        with:
          version: v2.1.0 # defaults to 'latest'
          legacy: true    # will also install in PATH as `docker-compose`
```
## Privacy

This Action contacts Chainguard's licensing server to verify authorization. Connection metadata (IP address, GitHub repository identifier, timestamp, and any metadata encoded in the auth token) is transmitted to Chainguard, Inc. even if authorization is denied in accordance with our [Privacy Notice](https://www.chainguard.dev/legal/privacy-notice)
