FROM docker.io/keeper/keeper-secrets-manager-tower-ee:1.4.0

USER root

# Install Kerberos + build dependencies
RUN dnf install -y \
    krb5-workstation \
    gcc \
    python3-devel \
    && dnf clean all

# Copy Python + Ansible requirements
COPY ee/requirements.txt /tmp/requirements.txt
COPY ee/requirements.yml /tmp/requirements.yml

# Install Python deps (with Kerberos)
RUN pip3 install --no-cache-dir -r /tmp/requirements.txt

# Install Ansible collections
RUN ansible-galaxy collection install -r /tmp/requirements.yml

# Copy Kerberos config
COPY ee/krb5.conf /etc/krb5.conf

# Validate modules
RUN python3 -c "import winrm, requests, pypsrp, requests_kerberos"

USER 1000
