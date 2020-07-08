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
				 def subList
				 def pathPart
				 def hasNext
				 list.each {if (it =~ /ADP/) {
					subList = it.split("/").toList()
					hasNext = true
					while(hasNext){
						pathPart = subList.last()
						
						if(pathPart =~ /Adp./){
							hasNext=false	
						}else{
							subList.remove(subList.size()-1)
						}
					}
					adpMap.put(subList.join("/"),"1")
				 }
				}
				Set depSet
				adpMap.each{entry -> depSet + getDependencies(entry.key)}
				depSet.each{println it}
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

def moveToTemp(String path){
	bat script: "Xcopy ${path} ../tempWorkspace /i /e /y"
	def javaPath = path+"../"+path.split("/").last()+".Java"
	bat script: "If Exist ${javaPath} Xcopy ${javaPath} ../tempWorkspace /i /e /y"
}

def getDependencies(String path){
	Set depSet
	def file = new File(WORKSPACE+"/"+path+"/.project") 
	def xml = new XmlParser().parseText(file.text)
	def subPath
	xml.getAt("projects").getAt("project").each{
		subPath = "/src/src/LIB/"+it.value().toString().replace("[","").replace("]","")
		depSet + subPath
		depSet + getDependencies(subPath)
	}
	return depSet
}