pipeline {
    // master executor should be set to 0
    agent any
	stages{
        stage('MVN-CodePackage-Jar') {
            steps {
                //sh
                bat "mvn clean package -DskipTests"
            }
        }
        stage('BuildImage-Docker') {
            steps {
                //sh
                bat "docker build -t=krupaautomation/selenium-docker-exec ."
            }
        }
        stage('PublishImage-Docker') {
            steps {
			    withCredentials([usernamePassword(credentialsId: 'krupadocker', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    //sh
			        bat "docker login --username=${user} --password=${pass}"
			        bat "docker push krupaautomation/selenium-docker-exec"
			    }                           
            }
        }
		stage('StartGrid-Docker'){
		steps{
			
			bat "docker compose up hub chrome firefox -d"
		}
		}
		stage('RunTest-Automation-Test'){
		steps{
			
			bat "docker compose up search-module book-flight"
		}
		}
		stage('StopGrid-Docker'){
		steps{
			
			bat "docker compose down"
		}
		}
    }
}