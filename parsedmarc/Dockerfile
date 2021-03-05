FROM python:alpine

ARG MAXMIND_LICENSE=XXX

RUN apk --no-cache update && \
    apk --no-cache add build-base libxml2-dev libxslt-dev curl && \
    pip install parsedmarc

WORKDIR /temp

RUN curl --output countrydb.tar.gz "https://download.maxmind.com/app/geoip_download?edition_id=GeoLite2-Country&license_key=${MAXMIND_LICENSE}&suffix=tar.gz" && \
    tar zxfv countrydb.tar.gz && \
    folder=$(find  -type d | grep -i geo) && \
    mkdir -p /usr/share/GeoIP && \
    mv $folder/*.mmdb /usr/share/GeoIP/GeoLite2-Country.mmdb

WORKDIR /
RUN rm -rf /temp

COPY parsedmarc.ini /parsedmarc.ini

# Create a group and user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Tell docker that all future commands should run as the appuser user
USER appuser

