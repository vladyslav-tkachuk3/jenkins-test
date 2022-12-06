FROM alpine

WORKDIR /myapp
COPY . /myapp

RUN apk add --no-cache python py-pip openssl ca-certificates
RUN apk add --no-cache --virtual build-dependencies python-dev build-base wget \
  && pip install -r requirements.txt \
  && python setup.py install \
  && apk del build-dependencies

CMD ["myapp", "start"]
