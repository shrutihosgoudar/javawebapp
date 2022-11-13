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
	  stage ('Unit Test'){
	   steps
            {
                       junit 'target/surefire-reports/*.xml'

            }
	  }
	  stage('testing'){
	    steps{
		  echo "THis is test job "
		}
		}
      stage('Producton'){
	    steps{
		  echo "This is Prd"
		  } 
		  }
	}
}
