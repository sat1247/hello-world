#!groovy

pipeline {

  parameters {
      string(name: "SLAVE_TARGET", defaultValue: "master", description: "slave machine that responsible to execute tasks")
	  string(name: "GIT_GOAL", defaultValue: "clone", description: "")
	  string(name: "GIT_REPO", defaultValue: "https://github.com/penumalla/appcode.git", description: "")
	  string(name: "GIT_BRANCH", defaultValue: "master", description: "")
	  string(name: "MVN_GOAL", defaultValue: "clean package", description: "")
	  string(name: "MVN_PROP", defaultValue: "-DskipTests", description: "")
	  string(name: "Ansible_Credentials", defaultValue: "ce1be52e-1e55-4672-a579-7c9d4a11931b", description: "")
  	  string(name: "Ansible_JobTemplate", defaultValue: "Test Tomcat", description: "")
          string(name: "Ansible_Servername", defaultValue: "TestAnsible", description: "")
	  
  }
  
   agent { label "${SLAVE_TARGET}" }
   

   
   stages {
      stage("scm-clone") {
	   when {
	      environment name: "GIT_GOAL", value: "clone"
	   }
	   steps {
	     script {
		   dir("D:/pipeline/rm/appcode"){
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
		  dir("D:/pipeline/rm/appcode") {
		    bat("git clean -fd")
		    bat("git pull origin master")
		    //bat("git pull")
		  }
		}
	  }
   
	stage("SCM-Compile/build")
	      {
		      steps {		      
	                  dir("D:/pipeline/rm/appcode")
		           {
		             //sh("mvn -P=Batch -Denv=qa clean package")
		               bat("cd")
		             // withMaven(maven:'Maven_3_3_9', mavenLocalRepo: '.repository',mavenSettingsConfig:'my-config') {
			      bat("mvn -Dmaven.repo.local=.repository ${MVN_GOAL} -Denv=local -P=Public")
				archive "./dol-public-web/target/*.war" 
		              bat("mvn -Dmaven.repo.local=.repository ${MVN_GOAL} -Denv=local -P=Idp")
			      archive "./dol-idp-web/target/*.war"
		           //   archiveArtifacts artifacts: 'dol-idp-web/target/*.war', onlyIfSuccessful: true

		             //    bat 'mvn -P=Batch -Denv=qa clean package'
		             //}
			   }  
			  /*  dir("D:/pipeline/rm/appcode")
		           {			   
		             //sh("mvn -P=Batch -Denv=qa clean package")
		               bat("cd")
		             // withMaven(maven:'Maven_3_3_9', mavenLocalRepo: '.repository',mavenSettingsConfig:'my-config') {
		              bat("mvn -Dmaven.repo.local=.repository ${MVN_GOAL} -Denv=local -P=Public")
				   archive "dol-public-web/target/*.war", onlyIfSuccessful: true
		             //    bat 'mvn -P=Batch -Denv=qa clean package'
		             //}
			   }  */
                  }
	      }
	  		 stage("Ansible Build")
     			 {
     				  steps {
           				ansibleTower(
					        towerServer: ${Ansible_Servername},
						ansibleTower credential: ${Ansible_Credentials},
          				        jobTemplate: ${Ansible_JobTemplate},
						importTowerLogs: true,
						inventory: '',						
					        jobTags: '',
   						limit: '',
						removeColor: false,
						verbose: false,
						extraVars: '''---
					)
  				       }
 			 }
   }
}
