# Stage 1: Base image
FROM traefik:v2.9

# Copy the Traefik configuration files based on the environment
# These can be overridden in the docker-compose.yml file based on the environment (local, dev, prod)
COPY traefik.local.yml /etc/traefik/traefik.yml

# Expose the necessary ports for Traefik
EXPOSE 80
EXPOSE 8080
EXPOSE 443

# Start Traefik with the config file specified
CMD ["--configFile=/etc/traefik/traefik.yml"]

