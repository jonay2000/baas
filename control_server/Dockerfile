FROM golang as build

WORKDIR /build

# Force modules
ENV GO111MODULE=on

# Cache dependencies
COPY go.* ./
RUN go mod download
RUN mkdir /static

# Build project
COPY . .
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -ldflags="-w -s" -o control_server_bin ./control_server

# Run stage
FROM scratch
WORKDIR /app

COPY --from=build /build/control_server_bin /app/control_server_bin
COPY --from=build /static /static

ENTRYPOINT ["/app/control_server_bin"]
