FROM golang:1.23.4-alpine3.21 AS build

WORKDIR /app

COPY go.mod go.sum ./

RUN go mod download

COPY . .

RUN go build -o vote-api

FROM alpine:3.21
COPY --from=build /app/vote-api /app/vote-api

WORKDIR /app

EXPOSE 8001
CMD ["./vote-api"]