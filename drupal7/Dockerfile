FROM solr:7.6-alpine

ENV SEARCH_API_SOLR_VER="7.x-1.14"
ENV SOLR_CONF_VER="7.x"

# Install curl and create config directory
USER root
RUN apk add --no-cache curl && \
    mkdir -v /drupalconfig && chown -v ${SOLR_USER}: /drupalconfig
USER solr


# Download config from drupal and mv files into place
RUN set -x;\
    curl -S "https://ftp.drupal.org/files/projects/search_api_solr-${SEARCH_API_SOLR_VER}.tar.gz" \
    | tar xvz search_api_solr/solr-conf/${SOLR_CONF_VER} && \
    pwd; ls -l ; \
    mv -v search_api_solr/solr-conf/${SOLR_CONF_VER}/* /drupalconfig && \
    rm -rfv search_api_solr

CMD solr-create -c collection1 -d /drupalconfig

HEALTHCHECK --start-period=60s --interval=120s --retries=3 CMD [ "curl","-fq","http://localhost:8983/solr/admin/cores?action=STATUS" ]
