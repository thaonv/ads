FROM ads/php7
#RUN rm -rf api-admin \
#&& git clone -b dev https://quocvuong92:s-z9a4dt1yoxWzPUesU9@gitlab.com/ftel-ads/api-admin.git \
#&& mv api-admin/.env.dev api-admin/.env \
#&& chmod -R 777 api-admin/storage 
#COPY . src/
#
#RUN chmod 777 -R src/vendor/

COPY . src/
RUN chmod -R 777 src/storage && \
chmod -R 777 src/vendor && \
rm -rf src/.env.dev && \
rm -rf src/Dockerfile && \
rm -rf src/kube/deployment.yaml
