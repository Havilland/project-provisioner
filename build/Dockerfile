FROM quay.io/operator-framework/ansible-operator:v0.8.1

USER 0

RUN yum --disableplugin=fastestmirror -y install git && yum clean all

USER 1001

COPY watches.yaml ${HOME}/watches.yaml

COPY roles/ ${HOME}/roles/
