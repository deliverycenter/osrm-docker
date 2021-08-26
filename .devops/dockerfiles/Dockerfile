FROM osrm/osrm-backend:latest

ARG OSRM_PBF_URL="http://download.geofabrik.de/south-america/brazil-latest.osm.pbf"
ARG OSRM_PROFILE

ENV OSRM_PBF_URL=$OSRM_PBF_URL

RUN apt-get update && \
    apt-get -y install curl

COPY ./profiles/*.lua /opt/

RUN curl -L $OSRM_PBF_URL --create-dirs -o /data/data.osm.pbf && \
    osrm-extract /data/data.osm.pbf -p /opt/$OSRM_PROFILE.lua && \
    osrm-contract /data/data.osrm

RUN useradd -m -s /bin/bash -d /opt app
RUN chown -R app:app /opt && chown -R app:app /data

USER app

EXPOSE 5000
CMD osrm-routed /data/data.osrm --max-table-size=8000
