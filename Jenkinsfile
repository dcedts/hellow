pipeline {

    agent any

    stages {
       stage('Build image') {
            sh "docker build --rm -t hello-nginx:latest ."
       }
	   
	   stage('Zip and upload') {
            sh "docker save hello-nginx:latest > /data1/docker/dockerimages/hello-nginx_latest.tar"
       }
       
       stage('Remove deployed image') {
			sh "docker rmi hello-nginx:latest --force"
			sh "docker system prune --force"
       }
    }
}
