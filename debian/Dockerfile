FROM node:slim

RUN groupadd -g 9999 app
RUN useradd -g app -s /bin/false -u 9999 app

RUN apt-get update
RUN apt-get install -y libusb-1.0-0-dev libudev-dev git make g++ python

RUN git clone https://github.com/OpenZWave/open-zwave.git
RUN cd open-zwave && make && make install
RUN ldconfig

RUN git clone https://github.com/mozilla-iot/gateway.git
WORKDIR /gateway
RUN npm i

RUN openssl genrsa -out privatekey.pem 2048
RUN openssl req \
  -new -key privatekey.pem \
  -out csr.pem -sha256 \
  -subj '/C=US/ST=CA/L=San Francisco/O=Docker/CN=Swarm Secret Example CA'
RUN openssl x509 -req -in csr.pem -signkey privatekey.pem -out certificate.pem
          
RUN chown -R app:app /gateway

EXPOSE 4443

CMD ["npm", "start"]
