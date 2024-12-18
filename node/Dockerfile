FROM node:16-slim

RUN apt-get update && apt-get install -y software-properties-common \
    poppler-utils \
    && add-apt-repository ppa:libreoffice/ppa \
    && apt-get install -y --force-yes libreoffice \
    && apt-get clean