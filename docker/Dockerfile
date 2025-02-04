# Build stage
ARG GO_VERSION=1.23.4
ARG APP_NAME=gokafkaexp

FROM  --platform=$BUILDPLATFORM golang:${GO_VERSION}-bookworm AS build

# Set environment variables
ENV CGO_ENABLED=1

# Create app directory and copy project to the app directory
RUN mkdir /app
ADD . /app

# Create and set working directory
WORKDIR /app

# Build the Go binary
RUN go build -v -mod=vendor -buildvcs=true -ldflags '-linkmode=external' -o ${APP_NAME} .

# Final stage - Minimal image with Go binary
FROM debian:latest

ARG APP_NAME

# Set environment variables
ENV APP_NAME=${APP_NAME}

# Copy the binary from the build stage
COPY --from=build /app/${APP_NAME} /usr/local/bin/${APP_NAME}

# Make the binary executable
RUN chmod +x /usr/local/bin/${APP_NAME}

# Expose necessary ports (you can modify as needed)
EXPOSE 8080
EXPOSE 8082

# Set the entrypoint for the container
ENTRYPOINT ["/usr/local/bin/${APP_NAME}"]
