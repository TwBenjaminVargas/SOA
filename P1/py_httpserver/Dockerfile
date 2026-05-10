FROM python:3.9.18-slim-bullseye
LABEL maintainer="Benjamin Vargas <bvargas161@alumnos.iua.edu.ar>"

WORKDIR /www

ARG UID=1000
ARG GID=1000

RUN apt-get update \
  && apt-get install -y --no-install-recommends curl \
  && rm -rf /var/lib/apt/lists/* \
  && groupadd -g "${GID}" ben \
  && useradd --create-home --no-log-init -u "${UID}" -g "${GID}" ben \
  && chown ben:ben -R /www

USER ben

ENV PYTHONUNBUFFERED="true" \
    PYTHONPATH="." \
    PATH="${PATH}:/home/ben/.local/bin" \
    USER="ben" \
    BEN_PORT="8080"

COPY --chown=ben:ben httpServer /usr/local/bin
COPY --chown=ben:ben index.html .

RUN chmod +x /usr/local/bin/httpServer

EXPOSE 8080

HEALTHCHECK --interval=30s --timeout=5s --retries=3 CMD curl -fsS http://localhost:$BEN_PORT/health || exit 1

CMD ["sh", "-c", "httpServer 0.0.0.0:$BEN_PORT /www"]
