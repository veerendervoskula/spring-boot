
node ("veer-jenkins-win") {
 
    def build_id = "${env.BUILD_ID}";
    def branch_name = "${env.BRANCH_NAME}";
    def environment = "${branch_name}" ;
    def project_key = "springboot" ;
    def lab = "2";
    def artifactory_server = Artifactory.server('https://vveerender.jfrog.io/');
    def uploadbuildInfo = "";
    def currentPath = "";
    
    stage ('Print Environment')
    {
        bat 'set'
    }


   stage ("Cleaning Workspace")
    {
        currentPath = pwd()
        println "currentPath : ${currentPath}"
        try{        
           
        'git clean -xffd'
         } catch (e) {                
                println "I caught exception while cleaning workspace: ${e.message}"
      }
    }

    stage ('Checking out Sourcecode to CI server')
    {
        checkout scm
    }

    stage ('Install Application dependencies')
    {
        bat 'mvn clean install'           
    }
  
    stage ('Build')
    {
       bat 'mvn package'        
    }
 
    stage('Upload artifacts to artifactory') 
    { 

        def uploadSpec = """{
            "files": [
                        {
                        "pattern": "target/gs-spring-boot-docker-0.1.0.jar",
                        "target": "spring-boot/",
                        "props": "type=jar;status=ready",
                        "flat":"true"
                        }
                    ]
        }"""
        uploadbuildInfo = artifactory_server.upload(uploadSpec)
    }
artifactory_server.publishBuildInfo(uploadbuildInfo)

}
