#!groovy

pipeline {

  parameters {
      string(name: "SLAVE_TARGET", defaultValue: "master", description: "slave machine that responsible to execute tasks")
	  string(name: "GIT_GOAL", defaultValue: "checkout", description: "")
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
	                     
			      bat("mvn -Dmaven.repo.local=D:/pipeline/rm/.repository ${MVN_GOAL} -Denv=local -P=Public")
				archive "./dol-public-web/target/*.war" 
				 // Stash that directory and file.
                             // Note that the includes could be "output/", "output/*" as below, or even
                           // "output/**/*" - it all works out basically the same.
                           stash name: "first-stash", includes: "D:/pipeline/rm/appcode/dol-public-web/target/*.war"
				
		           /*   bat("mvn -Dmaven.repo.local=.repository ${MVN_GOAL} -Denv=local -P=Idp")
			      archive "./dol-idp-web/target/*.war"*/
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
	   
	   
	   
	    stage("Send artifacts")
	       {
		       agent { label "${ansible_slave}" }		       
		       steps{
			       sh "ls -la {$pwd()}" 
			       
			       dir("/var/lib/awx/projects/tomcat/roles/tomcat/files"){
			       unstash "first-stash"
			       }
			       // Look, no output directory under the root!
                             // pwd() outputs the current directory Pipeline is running in.
			       sh "ls -la {$pwd()}"   
			       
			       // And look, output directory is there under first-stash!
                            //   sh "ls -la ${pwd()}/first-stash"
		    
		      /*steps {
			     
			withCredentials([[$class: 'SSHUserPrivateKeyBinding', credentialsId: '0ca1c416-5c49-43dd-844b-d3e28ce1d7ff']])
			      {
				sh ("echo %USERNAME%")
			       	sh ("C:/Users/svcwasadmin/.ssh")
				sh ("pwd")
				sh ("scp D:/pipeline/rm/appcode/dol-public-web/target/dol-public-web-0.0.1-SNAPSHOT.war svcwasadmin@labansible01:/var/lib/awx/projects/tomcat/roles/tomcat/files/dol-public-web-0.0.1-SNAPSHOT.war")       
      			       
			      }*/
		       
		       
		       
		       
			    //ws("D:/")
			   //   {
			   //    sshagent (credentials: ['0ca1c416-5c49-43dd-844b-d3e28ce1d7ff']) {
				       
				//    sh ("scp -r D:/pipeline/rm/appcode/dol-public-web/target/dol-public-web-0.0.1-SNAPSHOT.war svcwasadmin@labansible01:/var/lib/awx/projects/tomcat/roles/tomcat/files/dol-public-web-0.0.1-SNAPSHOT.war")   
				       
				         //sh ("uname")				       
				  //     sh ("ssh svcwasadmin@labansible01 uname -a")
				    //    sh ("ls -al")	
				 //      	sh "echo SSH_AUTH_SOCK=$SSH_AUTH_SOCK"
      				//	 sh "ls -al $SSH_AUTH_SOCK || true"
      					// sh "ssh -vvv -o StrictHostKeyChecking=no svcwasadmin@labansible01 uname -a"
				//         sh "ls -al"
				   //      sh "whoami"
				//         
                             //    sh 'ssh -o StrictHostKeyChecking=no root@labansible01 uname -a'  
				  // bat("pscp.exe D:/pipeline/rm/appcode/dol-public-web/target/dol-public-web-0.0.1-SNAPSHOT.war root@labansible01:/var/lib/awx/projects/tomcat/roles/tomcat/files")
			       //}
			      //}
			  
			  }
		   }
	  	/*	 stage("Ansible Build")
     			 {
     				  steps {
           				    ansibleTower (
							towerServer: 'TestAnsible',
							jobTemplate: 'Test Tomcat',
							importTowerLogs: true,
							inventory: 'Lab Inventory',
							jobTags: '',
							limit: '',
							removeColor: false,
							verbose: true,
							credential: 'Lab Credentials',
							extraVars: ''
					   )
  				       }
 			 } */
      }
}
