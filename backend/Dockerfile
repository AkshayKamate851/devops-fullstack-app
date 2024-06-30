# Use the official Golang image to create a build artifact
FROM golang:1.18 as builder

# Set the working directory inside the container
WORKDIR /app

# Copy the Go module files and download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy the rest of the application code
COPY . .

# Build the Go application
RUN go build -o main .

# Use a smaller image for the final container
FROM alpine:latest

# Set the working directory inside the container
WORKDIR /app

# Copy the binary from the builder stage
COPY --from=builder /app/main .

# Set environment variables
ENV DB_HOST="database-1.c10i8m2q0w9l.us-east-1.rds.amazonaws.com"
ENV DB_USER="admin"
ENV DB_PASSWORD="123456789"
ENV DB_NAME="database-1"
ENV DB_PORT="5432"

# Expose the port the app runs on
EXPOSE 8080

# Run the binary
CMD ["./main"]
