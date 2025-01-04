FROM golang:1.22 AS build

WORKDIR /app
COPY . .

RUN go build
RUN mkdir /bin/app
RUN mv vote-api /bin/app

FROM alpine:3.21
COPY --from=build /bin/app /bin/app

ARG PG_PASSWORD

ENV PG_URL="postgres://user:password@host.docker.internal:5432/postgres?sslmode=disable"

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

USER appuser

EXPOSE 8080
CMD ["/bin/app/vote-api"]