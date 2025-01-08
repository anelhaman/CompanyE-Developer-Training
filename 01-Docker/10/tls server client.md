## Using TLS to Secure Communication Between Docker Client and Daemon

To secure communication between the Docker client and the Docker daemon using TLS (Transport Layer Security), you need to set up SSL certificates for encrypted communication. This provides authentication to prevent unauthorized access.

### Steps to Set Up TLS for Docker

#### 1. Generate SSL Certificates
You need to generate a set of certificates (CA, server, and client certificates) for securing the connection.

Run the following commands using OpenSSL:

Generate the Certificate Authority (CA) private key
```bash
openssl genrsa -out ca-key.pem 2048
```

Generate the Certificate Authority (CA) public certificate
```bash
openssl req -new -x509 -key ca-key.pem -out ca.pem -days 3650
```

Generate the server's private key
```bash
openssl genrsa -out server-key.pem 2048
```

Generate the server's certificate signing request (CSR)
```bash
openssl req -new -key server-key.pem -out server.csr
```

Sign the server certificate with the CA
```bash
openssl x509 -req -in server.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem -days 3650
```

Generate the client's private key
```bash
openssl genrsa -out client-key.pem 2048
```

Generate the client certificate signing request (CSR)
```bash
openssl req -new -key client-key.pem -out client.csr
```

Sign the client certificate with the CA
```bash
openssl x509 -req -in client.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out client-cert.pem -days 3650
```

You will have the following files:
- `ca.pem`: The Certificate Authority's certificate (used to verify the other certificates)
- `server-cert.pem`: The Docker daemon’s server certificate
- `server-key.pem`: The Docker daemon’s private key
- `client-cert.pem`: The Docker client's certificate
- `client-key.pem`: The Docker client's private key

#### 2. Configure the Docker Daemon to Use TLS
Now, configure the Docker daemon to listen for secure connections (HTTPS) by specifying the paths to the generated certificates.

Edit the Docker daemon configuration file (`/etc/docker/daemon.json`):
```json
{
  "tls": true,
  "tlsverify": true,
  "tlscacert": "/path/to/ca.pem",
  "tlscert": "/path/to/server-cert.pem",
  "tlskey": "/path/to/server-key.pem",
  "hosts": ["tcp://0.0.0.0:2376"]
}
```

- `"tlsverify": true`: Enables verification of the client’s certificate.
- `"tlscacert"`: Path to the CA certificate.
- `"tlscert"`: Path to the server certificate.
- `"tlskey"`: Path to the server’s private key.
- `"hosts"`: This binds Docker to the specified TCP address and port (default is `2376`).

Restart Docker to apply the changes:
```bash
sudo systemctl restart docker
```

#### 3. Configure the Docker Client to Use TLS
Next, configure the Docker client to use the certificates to communicate with the Docker daemon over TLS.

Set the following environment variables to specify the paths to the certificates:
```bash
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://<docker-daemon-ip>:2376"
export DOCKER_CERT_PATH="/path/to/certs"
```

- `DOCKER_TLS_VERIFY=1`: Ensures that the Docker client will verify the server's certificate.
- `DOCKER_HOST`: Specifies the Docker daemon's IP address and port (`2376` is the default port for TLS-secured connections).
- `DOCKER_CERT_PATH`: Path to the directory containing the certificates: `client-cert.pem`, `client-key.pem`, and `ca.pem`.

#### 4. Verify the TLS Setup
To verify that the Docker client and daemon are communicating securely, run a simple Docker command such as:
```bash
docker info
```

If the connection is secure, you should see Docker daemon information without any warnings about insecure connections.

#### 5. Check the Active Docker Daemon Process for TLS
To ensure that the Docker daemon is running with TLS, inspect the daemon process:
```bash
ps aux | grep dockerd
```

You should see `--host=tcp://0.0.0.0:2376` in the command line, indicating that Docker is listening for TLS-secured connections.

#### 6. Testing and Troubleshooting
- Ensure both the client and server certificates are correctly placed and accessible.
- Ensure the firewall allows traffic on port `2376`.
- If using Docker Machine, ensure it is configured with the correct TLS settings.

### Conclusion
- The Docker daemon must be configured with SSL certificates and TLS to secure communication.
- The Docker client needs to use these certificates to connect securely.
- Enable TLS on both the client and the server to ensure encrypted communication and authentication.
