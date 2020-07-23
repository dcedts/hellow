pipeline {

	environment {
		containerName = "${SERVICE_NAME}"
		serviceName = "${SERVICE_NAME}"
		profileName = "${PROFILE}"
		imageName = "${profileName}_${SERVICE_NAME}"
		destination = "${SERVER}"
		portForward = "${PORT_FORWARD}"
        version = VersionNumber([
            versionNumberString :'${YEARS_SINCE_PROJECT_START}.${BUILD_MONTH}.${BUILDS_THIS_MONTH}',
            projectStartDate : '2020-07-01',
            versionPrefix : 'v'
        ])
	}

    agent any
	
    stages {
		stage('Check param') {
			steps {
				sh 'echo Container Name ${containerName}'
				sh 'echo Service Name ${serviceName}'
				sh 'echo Profile ${profileName}'
				sh 'echo Image Name ${imageName}'
				sh 'echo Destination ${destination}'
				sh 'echo Port Forward ${portForward}'
				sh 'echo Version ${version}'
			}
		}
		
		stage('Checkout') {
            steps {
				checkout scm
            }
        }
		
		stage('Create docker image') {
			steps {
				sh 'echo =====> CREATING DOCKER IMAGE WITH NAME: ${imageName}'
				sh 'docker build -t ${imageName} .'
			}
		}
	   
		stage('Deploy') {
			steps {
				sh 'echo ==============================================='
				sh 'echo    Deploy docker'
				sh 'echo ==============================================='
				sh 'docker save ${imageName} > /data1/docker/dockerimages/${imageName}.tar'
				sh "sh /data1/docker/dockerimages/copyImages.sh ${imageName} ${profileName} ${destination}"
				sh "ssh tomcat@${destination} 'sh /data1/docker/dockerdeploy/deployTest.sh ${profileName} ${containerName} ${imageName} ${portForward}'"
			}
		}
       
		stage('Remove deployed image') {
			steps {
				sh "docker rmi ${imageName} --force"
				sh "docker system prune --force"
			}
		}
    }
}
