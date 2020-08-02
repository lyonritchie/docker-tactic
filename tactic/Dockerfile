FROM python:3.7.8-buster

RUN apt update && apt install -y \
    apt-utils \
    unixodbc-dev \
    locales \
    locales-all

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US.UTF-8

# WORKDIR /tmp

COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

RUN git clone -b 4.7-updates --depth 1 https://github.com/Southpaw-TACTIC/TACTIC.git

RUN mkdir -p /opt/tactic



# ENTRYPOINT ["python3", "/opt/tactic/tactic/src/bin/startup_dev.py"]