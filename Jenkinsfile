pipeline {
    agent { label "iz1build1-0" }
    stages{
    	stage("npmBuild") {
    	 steps{     
	     withCredentials([
	   		    usernamePassword(
				credentialsId: "github-ci-serviceid-token",
				usernameVariable: "githubUsername",
				passwordVariable: "githubToken"
			),
		
	    	])
	    	 {
	    	   script {  
	    	   try {
				        sh "npm -v"
				        sh "rm -rf common-react-libraries || true"
				        sh 'git clone https://${githubToken}@github.ibm.com/maas360/common-react-libraries.git'
				        sh 'git config --global user.email "${GITHUB_USERNAME}"'
				    	sh 'git config --global user.name "Integration Integration"'
					
					    dir('common-react-libraries')
				    	{
				    	       sh 'rm -rf package-lock.json'
                           		       sh 'npm cache verify'
                                               sh 'npm install node-sass'
                                               sh 'npm install'
					       sh 'npm run build'
                        }
		     } catch (err) {
			            throw err
		                    }
	    	    }
	    	 }
	         }
    	}
    	stage("Registry Publish") {
    	     steps {  
	 	         
	    	        
	 	             script{
	 	                 try { 
	 	                    dir('common-react-libraries')   
	 	                    {
	 	                        sh 'npm config set always-auth=true'
	 	                        sh 'npm publish'
	 	                    } 
				  
					} catch (err) {
				
			    	throw err
		        	}
	 	         }
	    	  
	         }
    	}
    }
}
