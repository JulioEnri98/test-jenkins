pipeline {
	agent any
	tools{
		maven "Maven 3.8.6"
	}
	
	stages{
 		stage('Build artifact'){
			steps{
			sh "mvn clean package -DskipTests=true"
			archive 'target/*.jar'
			}
		}
		stage('Test Maven - JUnit'){
			steps{
			sh "mvn test"
			}
			post{
				always{
					junit 'target/surefire-reports/*.xml'
				}
			}
		}
		stage('Sonarqube Analysis - SAST'){
			steps{
				withSonarQubeEnv ('SonarQube'){
			sh "mvn sonar:sonar \
					-Dsonar.projectKey=maven-jenkins-pipeline \
				-Dsonar.host.url=http://52.87.158.127:9000"
				}
			}
		}
		stage('Wait for SonarQube'){
			steps{
				timeout(time:2, unit: 'MINUTES'){
					script{
						waitForQualityGate abortPipeline:true
					}
				}
			}
		}
	}
}
