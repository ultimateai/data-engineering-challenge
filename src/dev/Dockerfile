FROM ubuntu:bionic

RUN : \
    && apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        software-properties-common \
    && add-apt-repository -y ppa:deadsnakes \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        python3.8 && apt-get install -y python3-pip \
    && apt-get install -y wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && :

ENV PYTHONFAULTHANDLER=1 \
  PYTHONUNBUFFERED=1 \
  PYTHONHASHSEED=random

RUN add-apt-repository -y ppa:openjdk-r/ppa \
    && apt-get update && apt install -y openjdk-11-jdk

ENV DAEMON_RUN=true
ENV SPARK_VERSION=3.3.3
ENV HADOOP_VERSION=3
ENV SPARK_HOME=/spark

RUN wget --no-verbose https://dlcdn.apache.org/spark/spark-${SPARK_VERSION}/spark-${SPARK_VERSION}-bin-hadoop3.tgz \
      && tar -xvzf spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz \
      && mv spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION} spark \
      && rm spark-${SPARK_VERSION}-bin-hadoop${HADOOP_VERSION}.tgz

COPY /stream /stream
COPY /utils /utils
COPY /config /config
COPY ../twitter_stream_simulator.py .
COPY main.py .
COPY requirements.txt .
COPY submit.sh .

RUN pip install -r requirements.txt
RUN chmod +x submit.sh

CMD ["submit.sh"]
