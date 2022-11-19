pipeline {

    // run on jenkins nodes tha has slave label .....

    agent { label 'maven' }

    
    stages {
        
        stage('Build') {
            steps {
                // Run the maven build
                sh 'mvn clean install'
                
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
               withSonarQubeEnv('sonarqube') {
               
                    // some block
                    sh """
                    ${sonarscanner}/bin/sonar-scanner -Dsonar.projectKey=develop -Dsonar.java.binaries=**/*                                  
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
                nexusArtifactUploader artifacts: [[artifactId: 'SimpleWebApplication', classifier: '', file: 'target/SimpleWebApplication.war', type: '.war']], credentialsId: 'nexus', groupId: 'com.maven.bt', nexusUrl: 'http://172.31.7.54:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'http://172.31.7.54:8081/repository/maven-jenkins-releases/', version: '9.1.14'
            }
        }
    }
    }
