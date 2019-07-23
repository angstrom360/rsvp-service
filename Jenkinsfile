node {
    checkout scm
    docker.image('mysql:5').withRun('-e "MYSQL_ROOT_PASSWORD=root"') { c ->
        docker.image('mysql:5').inside("--link ${c.id}:db") {
          /* Wait until mysql service is up */
        }
        docker.image('centos:7').inside("--link ${c.id}:localhost") {
          /*
           * Run some tests which require MySQL, and assume that it is
           * available on the host name `db`
           */
          sh 'make check'
        }
  }
}

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