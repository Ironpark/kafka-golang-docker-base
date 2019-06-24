# kafka-golang-docker-base

```Dockerfile
#=================================
# image build with base image
#=================================
FROM ironpark/confluent-kafka-go:build as application
COPY . /app
WORKDIR /app
RUN go build -o /bin/app

# final stage
FROM ironpark/confluent-kafka-go:image
COPY --from=application /bin/app /bin/app
CMD /bin/app
```