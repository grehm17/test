pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
				script{
				 GIT_PREVIOUS_COMMIT=$(git rev-parse --short "HEAD^")
				 GIT_COMMIT=$(git rev-parse --short HEAD)
              changedFiles = bat(returnStdout: true, script: 'git diff --name-only $GIT_PREVIOUS_COMMIT $GIT_COMMIT').trim()
			  println(changedFiles)
             
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