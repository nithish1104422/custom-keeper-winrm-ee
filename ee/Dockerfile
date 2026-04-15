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

RUN mkdir -p /opt/atge-certs

# Copy certs to custom path
COPY ee/ATGE-root-CA.pem /opt/atge-certs/
COPY ee/ATGE-intermediate-CA.pem /opt/atge-certs/

# Also install certs into system trust store
COPY ee/ATGE-root-CA.pem /etc/pki/ca-trust/source/anchors/
COPY ee/ATGE-intermediate-CA.pem /etc/pki/ca-trust/source/anchors/

RUN update-ca-trust extract

# Create merged custom CA bundle
RUN cat /etc/pki/tls/certs/ca-bundle.crt \
    /opt/atge-certs/ATGE-root-CA.pem \
    /opt/atge-certs/ATGE-intermediate-CA.pem \
    > /opt/atge-certs/atge-ca-bundle.pem

ENV REQUESTS_CA_BUNDLE=/opt/atge-certs/atge-ca-bundle.pem
ENV SSL_CERT_FILE=/opt/atge-certs/atge-ca-bundle.pem

RUN pip3 install --no-cache-dir -r /tmp/requirements.txt
RUN ansible-galaxy collection install -r /tmp/requirements.yml

RUN python3 -c "import winrm, requests, pypsrp, requests_kerberos"

# Force certifi to use the same merged CA bundle
RUN CERT_PATH=$(python3 -c "import certifi; print(certifi.where())") && \
    echo "Certifi path: $CERT_PATH" && \
    cp /opt/atge-certs/atge-ca-bundle.pem "$CERT_PATH"

RUN echo "EE_BUILD_MARKER=ca-v7" > /etc/ee-build-marker

USER 1000
