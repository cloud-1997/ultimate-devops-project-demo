# Use the official Golang image as the base image
FROM golang:1.22 AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy the Go modules manifests
COPY go.mod go.sum ./

# Download Go modules
RUN go mod download

# Copy the source code
COPY . .

# Tidy up Go modules
RUN go mod tidy

# Build the Go application
RUN go build -o product-catalog main.go

# Use a minimal base image for the final container
FROM gcr.io/distroless/base-debian11

# Set the working directory inside the container
WORKDIR /app

# Copy the compiled binary from the builder stage
COPY --from=builder /app/product-catalog .

# Expose the port the application runs on
EXPOSE 8080

# Command to run the executable
CMD ["/app/product-catalog"]