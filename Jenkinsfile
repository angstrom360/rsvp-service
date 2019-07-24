pipeline{
	agent{
		docker{
			image 'maven:3-alpine'
			args '-v /d/JAVA_BOOTCAMP/JENKINS/:/d/JAVA_BOOTCAMP/JENKINS/'
		}
	}
	stages {
		stage('Build'){
			steps{
				sh 'mvn -B -DskipTests clean package'
			}
		}
		stage('Test'){
			steps{
				sh 'mvn test'
			}
			post{
				always{
					junit 'target/surefire-reports/*.xml'
				}
			}
		}
		stage('Deliever'){
			steps{
				sh './jenkins/scripts/deliver.sh'
			}	
		}		
		
	}
}