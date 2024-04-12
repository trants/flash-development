FROM golang:1.22-alpine
LABEL maintainer="Son Tran Thanh <286.trants@gmail.com>"

# Set the current working directory inside the container
WORKDIR /var/www

# Initialize dependency
RUN go mod init github.com/trants/go-base-apis

# Download dependencies
RUN go get -u github.com/cosmtrek/air
RUN go install github.com/cosmtrek/air

# Expose port 8080 to the outside world
EXPOSE 8080

# Command to run the executable
ENTRYPOINT ["air"]
