# kafka-golang-docker-base

this is go and rdlibkafka pre-buid docker image
https://hub.docker.com/r/ironpark/confluent-kafka-go

## Tag Rule
go`version`-`librdkafka version`

### Tags
`go1.12-1.2.2`,`go1.13-1.2.2`,`go1.14-1.2.2`,
`go1.12-1.3.0`,`go1.13-1.3.0`,`go1.14-1.3.0`,
`go1.12-1.4.0`,`go1.13-1.4.0`,`go1.14-1.4.0`,

## How To Use
```Dockerfile
#=================================
# image build with base image
#=================================
FROM ironpark/confluent-kafka-go:go1.14-1.4.0 as application
COPY . /app
WORKDIR /app
RUN go build -o /bin/app

# final stage
FROM alpine

COPY --from=ironpark/confluent-kafka-go:go1.14-1.4.0 /usr/lib/librdkafka.so.1 /usr/lib/librdkafka.so.1
COPY --from=ironpark/confluent-kafka-go:go1.14-1.4.0 /lib /lib
RUN mkdir /lib64 && ln -s /lib/libc.musl-x86_64.so.1 /lib64/ld-linux-x86-64.so.2

COPY --from=application /bin/app /bin/app
CMD /bin/app
```