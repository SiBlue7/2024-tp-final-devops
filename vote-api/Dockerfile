FROM golang:1.22 AS build

WORKDIR /app
COPY . .

RUN go build
RUN mkdir /bin/app
RUN mv vote-api /bin/app

FROM alpine:3.21
COPY --from=build /bin/app /bin/app

ARG PG_PASSWORD

ENV PG_URL="postgresql://neondb_owner:password@ep-hidden-mountain-a53rwzo1.us-east-2.aws.neon.tech/neondb?sslmode=require"

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

USER appuser

EXPOSE 8080
CMD ["/bin/app/vote-api"]