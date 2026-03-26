FROM docker.io/keeper/keeper-secrets-manager-tower-ee:1.4.0

USER root

COPY ee/requirements.txt /tmp/requirements.txt
COPY ee/requirements.yml /tmp/requirements.yml

RUN pip3 install --no-cache-dir -r /tmp/requirements.txt

RUN ansible-galaxy collection install -r /tmp/requirements.yml

RUN python3 -c "import winrm, requests, pypsrp"

USER 1000
