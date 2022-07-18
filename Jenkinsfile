pipeline {
    agent any
    tools {
        maven 'maven'
    }

	environment {
		GIT_CREDENTIALS = credentials('git-cli')
	}
    
    parameters {
        choice(
        	name: 'envSelected',
        	choices: ['LOCALHOST', 'DEV'],
        	description: 'Please select environment to deply on'
        )
    }
    
    stages {
    	stage('Clone Project') {
    	    steps {
    	    	sh 'rm -rf hello-jenkins'

				dir("hello-jenkins") {
					git changelog: false, branch: 'main', credentialsId: 'git-cli', url: 'https://github.com/BhaveshLakhpati/hello-jenkins.git'
				}
    	    }
    	}
    
    	stage('Build Application') {
    		steps {
	    		dir("hello-jenkins") {
					sh 'pwd'
					sh 'ls -l'
					
					script {
						sh 'mvn --version'
						
						sh 'mvn -DskipTests=true clean package'	
					}
	    		}
    		}
		}

    	stage('Run Spring Boot Application'){
			steps {
			    script {
			    	environment {
						ENV=params.envSelected.toLowerCase()
					}
			 		
					echo '${ENV}'

			 		ansiblePlaybook installation: 'ansible2', inventory: '$ENV.inv', playbook: 'ansible.yaml', disableHostKeyChecking: true       
			    }
			}
    	}
    }
}