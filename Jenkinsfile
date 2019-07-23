node {
    checkout scm
    /*
     * In order to communicate with the MySQL server, this Pipeline explicitly
     * maps the port (`3306`) to a known port on the host machine.
     */
    docker.image('mysql:5').withRun('-e "MYSQL_ROOT_PASSWORD=my-secret-pw" -p 3306:3306') { c ->
        /* Wait until mysql service is up */
        sh 'while ! mysqladmin ping -h0.0.0.0 --silent; do sleep 1; done'
        /* Run some tests which require MySQL */
        sh 'make check'
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