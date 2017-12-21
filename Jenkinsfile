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
              //sh 'mvn test'
              //step([$class: 'JUnitResultArchiver', testResults: 'target/surefire-reports/*.xml'])
              parallel (
                "Firefox" : {
                  junit 'target/surefire-reports/*.xml'
                },
                "Chrome" : {
                  junit 'target/surefire-reports/*.xml'
                }
                )
            }
            post {
                always {
                  junit 'target/surefire-reports/*.xml'
                  // Publish Reports
                  step([$class: 'XUnitBuilder',
                  thresholds: [[$class: 'FailedThreshold', unstableThreshold: '1']],
                  tools: [[$class: 'JUnitType', pattern: 'target/surefire-reports/**']]])

                  publishHTML([
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true, reportDir: 'target/surefire-reports',
                    reportFiles: '*.xml',
                    reportName: 'Coverage Report',
                    reportTitles: '']
                  )
                  //step([$class: 'JUnitResultArchiver', testResults: '/target/surefire-reports/*.xml'])
                  emailext attachLog: true, body: 'This is a test Job ', subject: 'Passed', to: 'sprasad.tech812@gmail.com'
                }

                failure {
					               //junit '**/target/surefire-reports/*.xml'
                        emailext attachLog: true, body: ' This is a test', subject: 'Failures', to: 'sprasad.tech812@gmail.com'

				}


            }
        }
        stage('Example') {
          steps {
            if (env.BRANCH_NAME == 'master') {
                echo 'I only execute on the master branch'
            } else {
              s  echo 'I execute elsewhere'
            }
          }

      }

    }
}
