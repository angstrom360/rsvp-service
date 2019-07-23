pipeline{
	agent{
		docker{
			image 'maven:3-alpine'
			args '-v /root/.m2:/root/.m2'
		}
	}
	stages {
		stage('Build'){
			steps{
				sh 'mvn -B -DskipTests clean package'
			}
		}
		stage('Test'){
			agent{
				docker{
					image 'mysql/mysql-server'
					args '--name some-mysql -e MYSQL_ROOT_PASSWORD=root -d'}
			}		
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