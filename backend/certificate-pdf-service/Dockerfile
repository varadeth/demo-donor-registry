############################
# STEP 1 build executable binary
############################
FROM golang:alpine AS builder
RUN apk update && apk add --no-cache 'git=~2'
ENV GO111MODULE=on
WORKDIR $GOPATH/src/packages/certificate-pdf/
COPY . .
RUN go get -d -v
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o /go/main .

############################
# STEP 2 build a small image
############################
FROM alpine:3

WORKDIR /

# Copy our static executable.
COPY --from=builder /go/main /go/main
COPY fonts /go/fonts
ENV PORT 8003
ENV GIN_MODE release
EXPOSE 8003

WORKDIR /go

# Run the Go Gin binary.
ENTRYPOINT ["/go/main"]
