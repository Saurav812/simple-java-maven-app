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
                              unstableThreshold: '1']],tools: [[$class: 'JUnitType', pattern: 'target/surefire-reports/**']]])
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
          //  post {
              //  always {
                //    step([$class: 'XUnitBuilder',thresholds: [ [$class: 'SkippedThreshold', failureThreshold: '0'], [$class: 'FailedThreshold', failureThreshold: '10']], tools: [[$class: 'JUnitType', pattern: 'reports/**']]])step([$class: 'XUnitBuilder',thresholds: [ [$class: 'SkippedThreshold', failureThreshold: '0'], [$class: 'FailedThreshold', failureThreshold: '10']], tools: [[$class: 'JUnitType', pattern: 'reports/**']]])
                //  junit 'target/surefire-reports/*.xml'
                  // Publish Reports
                //  step([$class: 'XUnitBuilder',
                //  thresholds: [[$class: 'FailedThreshold', unstableThreshold: '1']],
                //  tools: [[$class: 'JUnitType', pattern: 'target/surefire-reports/**']]])

                //  publishHTML([
                //    allowMissing: true,
                  //  alwaysLinkToLastBuild: true,
                  //  keepAll: true, reportDir: 'target/surefire-reports',
                //    reportFiles: '*.xml',
                  //  reportName: 'Coverage Report',
                //    reportTitles: '']
                //  )
                  //step([$class: 'JUnitResultArchiver', testResults: '/target/surefire-reports/*.xml'])
              //    emailext attachLog: true, body: 'This is a test Job ', subject: 'Passed', to: 'sprasad.tech812@gmail.com'
                //      }

              //  failure {
					               //junit '**/target/surefire-reports/*.xml'
              //          emailext attachLog: true, body: ' This is a test', subject: 'Failures', to: 'sprasad.tech812@gmail.com'

				        //        }
                //  }
        //}

      //  post {
      //    cleanWs cleanWhenAborted: false, cleanWhenFailure: false, cleanWhenNotBuilt: false, cleanWhenUnstable: false
      //  }
}
}
