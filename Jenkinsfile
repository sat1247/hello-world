#!groovy

pipeline {

  parameters {
      string(name: "SLAVE_TARGET", defaultValue: "master", description: "slave machine that responsible to execute tasks")
	  string(name: "GIT_GOAL", defaultValue: "clone", description: "")
	  string(name: "GIT_REPO", defaultValue: "https://github.com/penumalla/appcode.git", description: "")
	  string(name: "GIT_BRANCH", defaultValue: "master", description: "")
	  string(name: "MVN_GOAL", defaultValue: "clean package", description: "")
	  string(name: "MVN_PROP", defaultValue: "-DskipTests", description: "")
	  
  }
  
   agent { label "${SLAVE_TARGET}" }
   

   
   stages {
      stage("scm-clone") {
	   when {
	      environment name: "GIT_GOAL", value: "clone"
	   }
	   steps {
	     script {
		   dir("D:/pipeline/rm"){
		   deleteDir()    
		   if ("${GIT_BRANCH}" != ""){
		     		 
	            bat("git clone -b ${GIT_BRANCH} ${GIT_REPO}")
		    // bat("git clone -b ${GIT_BRANCH} credentialsId: 'd6a6ed5b-498f-47ed-b32e-969811c40abb' url: ${GIT_REPO}")
		   
		   }
		   else {
		     bat("git clone ${GIT_REPO}")
		   }
		   }
		 }
	   }
	  }
	  stage("SCM-Checkout") {
	    when {
		  environment name: "GIT_GOAL", value: "checkout"
		}
		steps {
		  dir("D:/pipeline/rm") {
		    bat("git clean -fd")
		    bat("git pull origin master")
		    //bat("git pull")
		  }
		}
	  }
   }
	stage("SCM-Compile/build")
	      {
		      steps {		      
	                  dir("D:/pipeline/rm")
		           {
		             //sh("mvn -P=Batch -Denv=qa clean package")
		               bat("cd")
		             // withMaven(maven:'Maven_3_3_9', mavenLocalRepo: '.repository',mavenSettingsConfig:'my-config') {
		              bat("mvn -Dmaven.repo.local=.repository ${MVN_GOAL} -Denv=local -P=Idp")
		             //    bat 'mvn -P=Batch -Denv=qa clean package'
		             //}
			   }  
                  }
	      }

}
