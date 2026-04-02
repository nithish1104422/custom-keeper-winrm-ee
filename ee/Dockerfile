FROM docker.io/keeper/keeper-secrets-manager-tower-ee:1.4.0

USER root

RUN dnf install -y \
    krb5-workstation \
    krb5-devel \
    gcc \
    python3-devel \
    && dnf clean all

COPY ee/requirements.txt /tmp/requirements.txt
COPY ee/requirements.yml /tmp/requirements.yml
COPY ee/krb5.conf /etc/krb5.conf

RUN pip3 install --no-cache-dir -r /tmp/requirements.txt
RUN ansible-galaxy collection install -r /tmp/requirements.yml

RUN python3 -c "import winrm, requests, pypsrp, requests_kerberos"

USER 1000
