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
              step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/*.xml'])
            }
            post {
                always {
                  junit '**target/surefire-reports/*.xml'
                  step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/*.xml'])
                  publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: '**/target/surefire-reports/*.xml', reportFiles: '**/target/surefire-reports/*.xml', reportName: 'HTML Report', reportTitles: ''])



				  emailext attachLog: true, body: 'This is a test Job ', subject: 'Passed', to: 'sprasad.tech812@gmail.com'
                }

				failure {
					junit '**target/surefire-reports/*.xml'
					emailext attachLog: true, body: ' This is a test', subject: 'Failures', to: 'sprasad.tech812@gmail.com'

				}


            }
        }

    }
}
