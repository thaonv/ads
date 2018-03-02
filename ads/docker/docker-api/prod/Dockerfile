FROM adsplayhubdocker.me/ads_php71

WORKDIR /src

COPY . .
RUN composer update && \
php artisan migrate && \
chmod -R 777 storage && \
chmod -R 777 vendor && \
rm -rf .env.dev && \
rm -rf .env.production && \
rm -rf .env.example && \
rm -rf .env.locnt12 && \
rm -rf .env && \
rm -rf Dockerfile && \
rm -rf kube/deployment.yaml
