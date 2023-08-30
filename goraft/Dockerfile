# Build the Go app
FROM golang:1.17 as builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

# Start from the Redis base image
FROM redis:6.2

# Set work directory
WORKDIR /app

# Copy Go binary from the previous stage
COPY --from=builder /app/main /app/main

# Add an entry script to run both Redis and your Go app
COPY entry.sh /app/entry.sh
RUN chmod +x /app/entry.sh

CMD ["/app/entry.sh"]
