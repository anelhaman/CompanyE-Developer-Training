## Enabling Docker Content Trust (DCT) with Self-Hosted Docker Registry

If you're running your own Docker registry and want to enable Docker Content Trust (DCT), you need to use Notary to sign images. DCT ensures that only images with valid digital signatures can be pulled or pushed to a registry.

### 1. Install and Configure Docker Notary
Notary is the tool used to sign and verify Docker images, allowing you to use Docker Content Trust (DCT) with your self-hosted registry.

#### Steps to install Notary:
- Install Notary on your machine where the Docker registry is running.
```bash
curl -sSL https://github.com/docker/notary/releases/download/v0.6.1/notary-0.6.1-linux-amd64.tar.gz | tar -xz -C /usr/local/bin
```

- Verify that Notary has been installed successfully:
```bash
notary --version
```

### 2. Configure Docker Registry to Support Notary
If you're running Docker Registry using Docker, you need to modify the registry's configuration file (`config.yml`) to enable Notary support.

#### Example configuration for Docker Registry with Notary:
```yaml
notary:
  signer:
    # Enable Notary signer
    enabled: true
  storage:
    # Notary storage backend
    backend: "filesystem"
    filesystem:
      rootdir: "/var/lib/registry/notary"
```

### 3. Enable Docker Content Trust (DCT)
After configuring Notary and Docker Registry to support image signing, you need to enable Docker Content Trust (DCT) on the machine from which you'll pull or push Docker images.

#### Enable DCT by setting the environment variable `DOCKER_CONTENT_TRUST` to `1`:
```bash
export DOCKER_CONTENT_TRUST=1
```

### 4. Sign Docker Images
Once DCT and Notary are enabled, you can sign your Docker images before pushing them to the registry, or when pulling images, Docker will verify the signature.

- Use the `docker push` command to push signed images to your registry.
- Use the `docker pull` command to pull images from the registry, and DCT will verify the image signatures automatically.

### 5. Verify Signatures
After pushing signed images to your Docker registry with Notary, you can pull the images from the registry, and Docker will automatically verify the image signature using DCT.

### Conclusion
- **Notary** is required to enable Docker Content Trust (DCT) for private Docker registries.
- **Signing images** using Notary allows you to use DCT with your self-hosted registry.
- Enable DCT by setting the `DOCKER_CONTENT_TRUST=1` environment variable on the machine you use to pull or push images.
