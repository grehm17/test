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
				 def subMap = [:]
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
				adpMap.each{entry -> subMap = subMap + getDependencies(entry.key)}
				adpMap.each{entry -> moveToTemp(entry.key)}
				subMap.each{entry -> moveToTemp(entry.key)}
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
	def workspace = WORKSPACE
	bat script: "Xcopy ${workspace}/${path} ${workspace}/../tempWorkspace /i /e /y"
	if(path !=~ /.Java/){
		def javaPath = path+"../"+path.split("/").last()+".Java"
		bat script: "If Exist ${workspace}/${javaPath} Xcopy ${workspace}/${javaPath} ${workspace}/../tempWorkspace /i /e /y"
	}
}

def getDependencies(String path){
	def depMap  = [:]
	def file = new File(WORKSPACE+"/"+path+"/.project") 
	def xml = new XmlParser().parseText(file.text)
	def subPath
	def depList = xml.getAt("projects").getAt("project")
	if (depList.size()>0){
	depList.each{
		subPath = "/src/src/LIB/"+it.value().toString().replace("[","").replace("]","")
		depMap.put(WORKSPACE+subPath,"1")
		depMap = depMap + getDependencies(subPath)
	}}
	return depMap
}