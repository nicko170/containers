FROM quay.io/ansible/awx-ee:latest
ARG TARGETPLATFORM
ARG BUILDPLATFORM
ARG VERSION
ARG CHANNEL

USER root

RUN pip install arouteserver==1.1.0 && curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python

ENV PATH "$HOME/.local/bin/:$HOME/.poetry/bin:$PATH"

RUN arouteserver setup --dest-dir ~/arouteserver

COPY ./apps/awx-ee/bgpq3-0.1.36.1-1.el8.x86_64.rpm /tmp/bgpq3-0.1.36.1-1.el8.x86_64.rpm
RUN yum install -y /tmp/bgpq3-0.1.36.1-1.el8.x86_64.rpm && rm -f /tmp/bgpq3-0.1.36.1-1.el8.x86_64.rpm

COPY ./apps/awx-ee/bgpq4-1.6-1.el8.x86_64.rpm /tmp/bgpq4-1.6-1.el8.x86_64.rpm
RUN yum install -y /tmp/bgpq4-1.6-1.el8.x86_64.rpm && rm -rf /tmp/bgpq4-1.6-1.el8.x86_64.rpm
