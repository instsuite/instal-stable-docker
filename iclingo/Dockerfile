FROM python:3.5.3

RUN apt-get update && apt-get install -y \
libpq-dev \
libstdc++6 \
libgcc-4.9-dev \
g++ \
gcc \
wget \
scons \ 
bison \
re2c

COPY ./clingo-5.1.0.tar.gz .

RUN  tar -xvzf clingo-5.1.0.tar.gz && cd clingo-5.1.0 && scons --build-dir=release

CMD ["echo", "Don't use this, extend instal-base"]