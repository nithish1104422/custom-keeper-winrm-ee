FROM docker.io/keeper/keeper-secrets-manager-tower-ee:1.4.0

USER root

RUN dnf install -y \
    krb5-workstation \
    krb5-devel \
    gcc \
    python3-devel \
    ca-certificates \
    && dnf clean all

COPY ee/requirements.txt /tmp/requirements.txt
COPY ee/requirements.yml /tmp/requirements.yml
COPY ee/krb5.conf /etc/krb5.conf
# Add internal CA certs for WinRM HTTPS validation
COPY ee/ATGE-root-CA.pem /etc/pki/ca-trust/source/anchors/
COPY ee/ATGE-intermediate-CA.pem /etc/pki/ca-trust/source/anchors/

RUN update-ca-trust extract

RUN pip3 install --no-cache-dir -r /tmp/requirements.txt
RUN ansible-galaxy collection install -r /tmp/requirements.yml

RUN python3 -c "import winrm, requests, pypsrp, requests_kerberos"

USER 1000
