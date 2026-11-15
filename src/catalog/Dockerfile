FROM golang:1.27 AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN go build -o main main.go

FROM gcr.io/distroless/base-debian12:nonroot

WORKDIR /app
COPY --from=builder /app/main ./
USER nonroot:nonroot

EXPOSE 8080
CMD [ "./main" ]