pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
				script{
				 GIT_PREVIOUS_COMMIT=bat(returnStdout: true, script: 'git rev-parse --short "HEAD^"')
				 GIT_COMMIT=bat(returnStdout: true, script: 'git rev-parse --short HEAD')
              
			  println(GIT_PREVIOUS_COMMIT)
			  println(GIT_COMMIT)
             
            }
        }}
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}