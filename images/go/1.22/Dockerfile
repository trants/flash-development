FROM golang:1.22-alpine
LABEL maintainer="Son Tran Thanh <286.trants@gmail.com>"

WORKDIR /var/www

RUN go mod init dummy

RUN go get github.com/githubnemo/CompileDaemon

ENTRYPOINT CompileDaemon --build="go build main.go" --command="./main"

VOLUME /var/www

EXPOSE 8080
