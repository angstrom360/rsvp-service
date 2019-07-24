node{
    Class.forName("com.mysql.jdbc.Driver")
    def sql = Sql.newInstance("jdbc:mysql://mysql:3306/rsvp_test", "root","root", "com.mysql.jdbc.Driver")
}
pipeline{
	agent{
		docker{
			image 'maven:3-alpine'
			args '-v /root/.m2:/root/.m2'
		}
	}
	stages {
		stage('SQL TEST'){
			node{
				Class.forName("com.mysql.jdbc.Driver")
				def sql = Sql.newInstance("jdbc:mysql://mysql:3306/rsvp_test", "root","root", "com.mysql.jdbc.Driver")
			}
				
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