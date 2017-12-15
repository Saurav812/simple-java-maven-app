#!groovy

node('master') {


    currentBuild.result = "SUCCESS"

    try {

       stage('Build') {
            
                sh 'mvn -B -DskipTests clean package'
        
		}


        stage('Test') {
            
              sh 'mvn test'
            
            post {
                always {
                  junit 'target/surefire-reports/*.xml'
				  emailext attachLog: true, body: '', subject: 'Failures', to: 'sprasad.tech812@gmail.com'
                }


            }
        }


    }
    catch (err) {

        currentBuild.result = "FAILURE"

            mail body: "project build error is here: ${env.BUILD_URL}" ,
            from: 'sprasad.tech812@gmail.com',
            replyTo: 'sprasad.tech812@gmail.com',
            subject: 'project build failed',
            to: 'sprasad.tech812@gmail.com'

        throw err
    }

}