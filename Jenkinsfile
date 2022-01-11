pipeline {
    // master executor should be set to 0
    agent any
	stages{
        stage('CodePackage') {
            steps {
                //sh
                bat "mvn clean package -DskipTests"
            }
        }
        stage('BuildImage') {
            steps {
                //sh
                bat "docker build -t=krupaautomation/selenium-docker-exec ."
            }
        }
        stage('PublishImage') {
            steps {
			    withCredentials([usernamePassword(credentialsId: 'krupadocker', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    //sh
			        bat "docker login --username=${user} --password=${pass}"
			        bat "docker push krupaautomation/selenium-docker-exec"
			    }                           
            }
        }
		stage('StartGrid'){
		steps{
			
			bat "docker compose up -d hub chrome firefox"
		}
		}
		stage('RunTest'){
		steps{
			
			bat "docker compose up search-module book-flight"
		}
		}
    }
	post{
		always{
			archiveArtifacts artifacts:"D:\\GIT\\Reports\\"
			bat "docker compose down"
		}
	}
}