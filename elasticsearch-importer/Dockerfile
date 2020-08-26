from taskrabbit/elasticsearch-dump

RUN apk add curl

COPY scripts/ scripts/
COPY data/ data/

RUN chmod +x scripts/waitfor 
RUN chmod +x scripts/entrypoint.sh
