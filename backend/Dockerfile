# Stage 1: Build (build environment) - Use the smallest images & delete unnecessary packages
FROM golang:1.22 as build

# Install necessary packages and update certificates
RUN apt-get update && \
    apt-get install -y ca-certificates && \
    rm -rf /var/lib/apt/lists/* # cleans up the package list to reduce the image size

COPY ./ca-certs/* /usr/local/share/ca-certificates/
RUN update-ca-certificates

# Sets the working directory inside the container
WORKDIR /usr/src/app

COPY go.mod go.sum ./
RUN go mod download && go mod verify

# Copies the entire application source code into the container
COPY . .
# builds the Go application, outputting the binary as /app
RUN go build -v -o /app .

# Stage 2: Final stage (runtime environment) - Use the smallest images & delete unnecessary packages
FROM debian:bullseye-slim

# Install necessary packages
RUN apt-get update && \
    apt-get install -y ca-certificates && \
    rm -rf /var/lib/apt/lists/* # cleans up the package list to reduce the image size

# Copy the binary from the build stage
COPY --from=build /app /usr/local/bin/app

# Expose container to a non-privileged port (port number higher than 1024)
EXPOSE 8080

# Set a non-root user - Use rootless container
RUN useradd -m -s /bin/sh aahalqarni
USER aahalqarni

CMD ["app"]
