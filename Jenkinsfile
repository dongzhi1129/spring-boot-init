def Gloable_Service_Name = "spring-boot-init"
def Gloable_User_Name="dongzhi1129"
def Gloable_version
pipeline {
   agent any
   parameters {
       string(name:'Git_Branch',defaultValue:'master',description:'Please enter git repo')
       string(name:'Git_Tag',defaultValue:'',description:'')
   }
   tools{
       gradle 'Gradle_6.6.1'
   } 

   stages {
   	  stage('init') {
   	  	steps{
   	  		script { 
   	  			sh 'mkdir -p ${WORKSPACE}/SCM'
   	  		}
   	  	}
   	  }
   
   
      stage('SCM') {
         steps { dir("${WORKSPACE}/SCM"){
         		script {
         			def repoUrl="https://github.com/${Gloable_User_Name}/${Gloable_Service_Name}.git"
		            git branch:params.Git_Branch,url:repoUrl
		            def commitId = sh returnStdout:true,script:'git rev-parse --short HEAD'
		            if(params.Git_Tag != "") {
		                 def Gloable_tag = params.Git_Tag
		                 sh "cd ${WORKSPACE}/SCM && git checkout ${Gloable_tag}"
		             }
		             Gloable_version = (params.Git_Tag != ""?params.Git_Tag:"latest")
		             print Gloable_version
         		}
             
             
         }
            
         }
      }

	  stage('buildImage'){
		  steps {dir("${WORKSPACE}/SCM")} {
			  script{
				  sh "cd ${WORKSPACE}/SCM && chmod 755 gradlew && ./gradlew -version"
				  sh "cd ${WORKSPACE}/SCM && ls -a && ./gradlew -Pversion= ${Gloable_version} -PimageRepo='snapshot' clean build dockerPush -x test"
			  }
		  }
	  }
   }
}