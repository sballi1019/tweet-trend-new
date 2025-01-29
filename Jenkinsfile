def registry = 'https://balli1910.jfrog.io/'
pipeline{
    agent{
      node{
           label 'maven'
    }
  }
  environment  {
        PATH= "/opt/apache-maven-3.9.9/bin:$PATH"
  }
   stages{
      stage("build stage"){
        steps {
          echo "build started........"
            sh 'mvn clean deploy'
          echo "build  complete"  
        }
      }
      
      stage("Jar Publish") {
            steps {
                script {
                        echo '<--------------- Jar Publish Started --------------->'
                         def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"jfrog-cred"
                         def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                         def uploadSpec = """{
                              "files": [
                                {
                                  "pattern": "jarstaging/(*)",
                                  "target": "libs-release-local/{1}",
                                  "flat": "false",
                                  "props" : "${properties}",
                                  "exclusions": [ "*.sha1", "*.md5"]
                                }
                             ]
                         }"""
                         def buildInfo = server.upload(uploadSpec)
                         buildInfo.env.collect()
                         server.publishBuildInfo(buildInfo)
                         echo '<--------------- Jar Publish Ended --------------->'  
                
                }
            }   
        }
   
   }
}
