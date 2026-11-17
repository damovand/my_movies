FROM --platform=$BUILDPLATFORM python:3.14-alpine AS builder

WORKDIR /movie-app

COPY requirements.txt /movie-app

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV FLASK_APP app.py
ENV FLASK_ENV development
ENV FLASK_RUN_HOST 0.0.0.0
ENV FLASK_RUN_PORT 8001
ENV OMDB_API_KEY d0f193ac

EXPOSE 8001

CMD ["flask", "run"]

FROM builder AS dev-envs

COPY --from=builder /movie-app /movie-app

RUN apk update && apk add --no-cache bash
RUN apk add --no-cache git

RUN addgroup -S docker
RUN adduser -S --shell /bin/bash --ingroup docker vscode

# install Docker tools (cli, buildx, compose)
COPY --from=gloursdocker/docker / /

CMD ["flask", "run"]