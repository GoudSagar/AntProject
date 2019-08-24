pipeline {
    agent any
      tools {  
        ant 'ANT-1.9'
        jdk 'JAVA_HOME'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "ANT_HOME = ${ANT_HOME}"
                '''
            }
        }
        stage ('Build') {
            steps {
                echo 'Building'
                sh 'ant -version'
                sh 'ant clean war'
            }
        }
	
	stage ('Test') {
	   steps {
                echo 'Running Junit Tests Cases'
                sh 'ant test'
             }
         }
       
       stage ('Sonar') {
             environment {
             scannerHome = tool 'SONAR_SCANNER'
             }
             steps {
                echo 'Running Static Code Analysis'
                 withSonarQubeEnv('Sonarqube') {
                 sh '${scannerHome}/bin/sonar-scanner'
                 }
            }
        }
       
      stage ('Nexus') {
             steps {
                   nexusArtifactUploader(
                   nexusVersion: 'nexus3',
                   protocol: 'http',
                   nexusUrl: '3.16.130.227:8081',
                   groupId: 'MY-POC',
                   version: '1.0-SNAPSHOT',
                   repository: 'maven-snapshots',
                   credentialsId: 'Nexus',
                       artifacts: [
                         [artifactId: 'POC-CI-CD',
                         type: 'war',
                         classifier: '',
                         file: '/var/lib/jenkins/workspace/TEST_PIPELINE/dist/AntExample.war']
                       ]
                   )
               }
          }
        
      stage('Deploy to test') {
             steps {
              echo 'Deploying in Test Node On Tomcat Server'
              ansiblePlaybook( 
                       playbook: 'site.yml',
                       inventory: '/etc/ansible/hosts', 
                       credentialsId: 'ansi', 
                       hostKeyChecking: 'false' )
               }       
            }        
        }
    }
       
  }
}  


