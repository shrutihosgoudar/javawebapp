pipeline {
  agent {
      label 'maven' 
  }
	stages{
	  stage ('Build'){
	    steps{
		sh '''
			mvn clean install
			'''
		}
	  }
	         stage('Unit Testing junit')
        {
            steps
            {
                       junit 'target/surefire-reports/*.xml'

            }
        }
        stage('Code Coverage')
        {
          steps
          {
         
           jacoco()
           }
        }
        
        stage('Code Quality Check (Sonarqube)')
        {
          steps
          {
             script
             {
               def sonarscanner = tool 'sonar_scanner'
               withSonarQubeEnv(credentialsId: 'sonar-jenkins') {
               
                    // some block
                    sh """
                    ${sonarscanner}/bin/sonar-scanner
                
                    """
                }
             }
          }
        }
        
        stage('Quality gate') {

            steps {

                timeout(time: 1, unit: 'MINUTES') {

                    retry(3) {

                        script {

                            def qg = waitForQualityGate()

                            if (qg.status != 'OK') {

                                error "Pipeline aborted due to quality gate failure: ${qg.status}"

                            }

                        }

                    }

                }

            }

        }

        stage('Upload to Nexus') {
            steps {
                // Deploy to Nexus
               nexusPublisher nexusInstanceId: 'nexus', nexusRepositoryId: 'EED_Engg-Excellence-Devops-POC_maven_releases', packages: []
            }
        }
    }
      stage('Producton'){
	    steps{
		  echo "This is Prd"
		  } 
		  }
	}
}
