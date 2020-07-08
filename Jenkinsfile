pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
				script{
				 GIT_PREVIOUS_COMMIT=bat(returnStdout: true, script: 'git rev-parse --short --names-only "HEAD^"')
				 GIT_COMMIT=bat(returnStdout: true, script: 'git rev-parse --short --names-only HEAD')
				 
				 list = GIT_PREVIOUS_COMMIT.readLines()
				 GIT_PREVIOUS_COMMIT = list[2]
				 
				 list = GIT_COMMIT.readLines()
				 GIT_COMMIT = list[2]
				 
				 changedFiles = bat(returnStdout: true, script: "git diff --name-only ${GIT_PREVIOUS_COMMIT} ${GIT_COMMIT}").trim()
				 def adpMap = [:]
				 list = changedFiles.readLines()
				 String[] subList
				 def pathPart
				 def hasNext
				 list.each {if (it =~ /ADP/) {
					subList = it.split("/")
					hasNext = true
					while(hasNext){
						pathPart = subList.last()
						
						if(! pathPart =~ /Adp/){
							subList.removeLast()
						}else{
							hasNext=false
						}
					}
					adpMap.put(subList.join("/"),"1")
				 }
				}
				adpMap.each{entry -> println entry.key}
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