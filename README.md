# Build nginx
docker build --rm -t hello-nginx:latest .

# Create Container
docker container create --name hello-nginx -p 80:80 hello-nginx:latest

# docker-compose up -d