FROM launcher.gcr.io/google/debian9 AS build

RUN apt-get update \
    && apt-get install -y --no-install-recommends gettext

ADD rtview-dataserver/chart /tmp/chart
RUN cd /tmp \
    && tar -czvf /tmp/rtview-dataserver.tar.gz chart

ADD schema.yaml /tmp/schema.yaml

# Provide registry prefix and tag for default values for images.
ARG REGISTRY
ARG TAG
RUN cat /tmp/schema.yaml \
    | env -i "REGISTRY=$REGISTRY" "TAG=$TAG" envsubst \
    > /tmp/schema.yaml.new \
    && mv /tmp/schema.yaml.new /tmp/schema.yaml


FROM gcr.io/cloud-marketplace-tools/k8s/deployer_helm:sha_a0248f39dd6663322f2d9c1c9de658e7c332fcea
COPY --from=build /tmp/rtview-dataserver.tar.gz /data/chart/
COPY --from=build /tmp/schema.yaml /data/
