FROM wal-g/golang:latest as build

WORKDIR /go/src/github.com/wal-g/wal-g

RUN apt-get update && \
    apt-get install --yes --no-install-recommends --no-install-suggests \
    liblzo2-dev

COPY vendor/ vendor/
COPY internal/ internal/
COPY cmd/ cmd/
COPY main/ main/

RUN cd main/pg && \
    go build -o wal-g -tags lzo -ldflags "-s -w -X main.BuildDate=`date -u +%Y.%m.%d_%H:%M:%S`"

FROM wal-g/ubuntu:latest

COPY --from=build /go/src/github.com/wal-g/wal-g/main/pg/wal-g /usr/bin

COPY docker/pg/scripts/ /tmp

CMD su postgres -c "/tmp/run_integration_tests.sh"
