pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
        }
		}
        stage('Test') {
            steps {
              sh 'mvn test'
            }
            post {
                always {
                  junit 'target/surefire-reports/*.xml'
				  emailext attachLog: true, body: 'This is a test', subject: 'Passed', to: 'sprasad.tech812@gmail.com'
                }
				
				failure {
					junit 'target/surefire-reports/*.xml'
					emailext attachLog: true, body: ' This is a test', subject: 'Failures', to: 'sprasad.tech812@gmail.com'
					
				}


            }
        }
		
    }
}
