# Use a lightweight base image
FROM alpine:latest

# Set PocketBase version as an argument (default: 0.22.8)
ARG PB_VERSION=0.22.8

# Install necessary packages: unzip, ca-certificates, and bash
RUN apk add --no-cache \
    unzip \
    ca-certificates \
    bash

# Download and unzip PocketBase
ADD https://github.com/pocketbase/pocketbase/releases/download/v${PB_VERSION}/pocketbase_${PB_VERSION}_linux_amd64.zip /tmp/pb.zip
RUN unzip /tmp/pb.zip -d /pb/

# Create the destination directories inside the container
RUN mkdir -p /pb/pb_migrations /pb/pb_hooks

# Use a shell command to copy the directories only if they exist locally
# Check if the `pb_migrations` folder exists locally and copy if it does
RUN if [ -d "./pb_migrations" ]; then cp -r ./pb_migrations /pb/; fi

# Check if the `pb_hooks` folder exists locally and copy if it does
RUN if [ -d "./pb_hooks" ]; then cp -r ./pb_hooks /pb/; fi

# Set the working directory to /pb (where PocketBase is installed)
WORKDIR /pb

# Expose the port PocketBase will run on (8090)
EXPOSE 8090

# Start PocketBase and bind to all IPs on port 8090
CMD ["./pocketbase", "serve", "--http=0.0.0.0:8090"]


# docker build -t dockerfile .
# docker run -d -p 8090:8090 dockerfile