pipeline {
    agent any
      stages {
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
                   nexusArtifactUploader {
                   nexusVersion('nexus3')
                   protocol('http')
                   nexusUrl('3.16.130.227:8081')
                   groupId('MY-POC')
                   version('1.0-SNAPSHOT')
                   repository('maven-snapshots')
                   credentialsId('bd3bd97d-5179-3f74-8953-9233527e11f1')
                       artifact {
                         artifactId('POC-CI-CD')
                         type('war')
                         classifier('debug')
                         file('/var/lib/jenkins/workspace/TEST_PIPELINE/dist/AntExample.war')
                       }
                   }
               }
          }
       }
   }  


