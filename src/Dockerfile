FROM docker.io/golang:1.22.3 as builder

WORKDIR /app
COPY ./go.mod ./go.sum ./
RUN go mod download

COPY * ./
RUN CGO_ENABLED=0 go build -o /output/gobin

FROM docker.io/alpine:latest

WORKDIR /application
COPY --from=builder /output/gobin .

ENTRYPOINT ["/application/gobin"]
