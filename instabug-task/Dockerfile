# Stage 1: Build stage
FROM golang:1.19 AS builder
WORKDIR /app-code
COPY go.mod go.sum ./
RUN go mod download 
COPY *.go ./
RUN CGO_ENABLED=0 GOOS=linux go build -o /goapp
# Stage 2: Setup stage "Last"
FROM alpine:latest
RUN adduser \
  --disabled-password \
  --gecos "" \
  --home "/nonexitstent" \
  --shell "/sbin/nologin" \
  --no-create-home \
  --uid "1002" \
  "goapp"
WORKDIR /app
COPY --from=builder ./goapp ./goapp
ENTRYPOINT ["./goapp"]
