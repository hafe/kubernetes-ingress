# syntax=docker/dockerfile:1.5
# this is here so we can grab the latest version of kind and have dependabot keep it up to date
FROM kindest/node:v1.30.2@sha256:ecfe5841b9bee4fe9690f49c118c33629fa345e3350a0c67a5a34482a99d6bba

# this is here so we can grab the latest version of skopeo and have dependabot keep it up to date
FROM quay.io/skopeo/stable:v1.15.2

FROM python:3.12@sha256:c7862834f921957523cc4dab6d7795a7a0d19f1cd156c1ecd3a3a08c1108c9a4

RUN apt-get update \
	&& apt-get install -y curl git \
	&& rm -rf /var/lib/apt/lists/*

WORKDIR /workspace/tests

COPY --link tests/requirements.txt /workspace/tests/
RUN pip install --require-hashes -r requirements.txt --no-deps

COPY --link deployments /workspace/deployments
COPY --link config /workspace/config

RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl \
	&& install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl \
	&& apt-get update && apt-get install -y apache2-utils

COPY --link tests /workspace/tests

COPY --link pyproject.toml /workspace/

ENTRYPOINT ["python3", "-m", "pytest"]
