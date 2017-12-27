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
                //sh 'mvn -B -DskipTests clean package'
                script {
                try {
                    // run tests in the same workspace that the project was built
                    echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                    dir ('test') {
                        sh 'mvn -B -DskipTests clean package'
                    }

                } catch (e) {
                    // if any exception occurs, mark the build as failed
                    currentBuild.result = 'FAILURE'
                    throw e
                } finally {
                    // perform workspace cleanup only if the build have passed
                    // if the build has failed, the workspace will be kept
                    //cleanWs cleanWhenFailure: false
                    emailext attachLog: true, body: 'Build has failed ', subject: 'FAILED', to: 'sprasad.tech812@gmail.com'
                }
        }
      }
		}

        stage('Test') {
            steps {
              script{
                  try {
                    echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                    sh 'mvn test'
                } catch (e) {
                    currentBuild.result = 'FAILURE'
                  throw e
                }

              }

              //step([$class: 'JUnitResultArchiver', testResults: 'target/surefire-reports/*.xml'])
              //parallel (
              //  "Firefox" : {
                //  junit 'target/surefire-reports/*.xml'
              //  },
              //  "Chrome" : {
              //    junit 'target/surefire-reports/*.xml'
                }
              //  )

            post {
                always {
                        //junit 'target/surefire-reports/*.xml'
                        script {
                            try {
                              step([$class: 'XUnitBuilder', thresholds: [[$class: 'FailedThreshold',
                              unstableThreshold: '1']],tools: [[$class: 'JUnitType', pattern: '/target/surefire-reports/**']]])
                            } catch (e) {
                                currentBuild.result = 'SUCCESS'
                              throw e
                            } finally {
                              emailext attachLog: true, body: 'Unit Test has passed ', subject: 'SUCCESS', to: 'sprasad.tech812@gmail.com'
                            }
                        }
                        publishHTML([
                          allowMissing: true,
                          alwaysLinkToLastBuild: true,
                          keepAll: true, reportDir: 'reports/',
                          reportFiles: '*.xml',
                          reportName: 'Coverage Report',
                          reportTitles: '']
                                    )

                        }
                  }

          }

          stage('Deploy to Tomcat') {
                steps {
                  script {
                        echo "Details of the ${env.tomcat_hostname}"
                        sshagent(['7c315e58-66b9-47bd-a49d-dc7c2cae1d98']) {
                        sh 'cp ${WORKSPACE}/target/my-app-1.0-SNAPSHOT.jar http://ec2-54-159-172-184.compute-1.amazonaws.com:8080/TOMCAT/webapps/'

                  }
                                                   }
                      }
                                    }
}
}
