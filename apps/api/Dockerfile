# Stage 1: Build the Go application
FROM golang:1.23 as builder

# Set the working directory inside the container
WORKDIR /app

# Define build arguments for the context paths
ARG SRC_PATH=.

# Copy go.mod and go.sum files to the working directory
COPY ${SRC_PATH}/go.mod ${SRC_PATH}/go.sum ./

# Download dependencies
RUN go mod download

# Copy the entire source code
COPY ${SRC_PATH}/ ./

# Build the Go application and output a binary named `api`
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o api main.go

# Stage 2: Create a lightweight image to run the application
FROM alpine:latest

RUN apk --no-cache add ca-certificates

LABEL author="Luke Oliff"

# Set the working directory inside the container
WORKDIR /root/

# Copy the built binary from the builder stage
COPY --from=builder /app/api .

# Command to run the application
CMD ["./api"]