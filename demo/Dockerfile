FROM golang:1.12-alpine as app
RUN apk add -U build-base git
COPY app /go/src/app
WORKDIR /go/src/app
ENV GO111MODULE=on
RUN go build -mod=vendor -a -v -tags 'netgo' -ldflags '-w -extldflags -static' -o docker-demo .

#FROM gcr.io/distroless/base:latest
FROM gcr.io/distroless/base-debian10:latest
COPY app/static /static
COPY --from=app /go/src/app/docker-demo /bin/docker-demo
COPY --from=busybox /bin/tar /bin/tar
COPY --from=busybox /bin/sh /bin/sh
COPY app/templates /templates
ENV COW_COLOR blue
EXPOSE 8080
ENTRYPOINT ["/bin/docker-demo"]
