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
       
       stage ('Clone Source Code') {
           steps {
              git branch: 'master',
              credentialsId: 'id',
              url: 'https://github.com/GoudSagar/AntProject.git'
           }
        }

        stage ('Build') {
            steps {
                echo 'Compiling and Building the Source Code'
                sh 'ant -version'
                sh 'ant clean war'
            }
        }
	
	stage ('Unit Test') {
	   steps {
                echo 'Running Unit Testing'
                sh 'ant test'
             }
         }
       
       stage ('Static Code Analysis') {
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
    
      stage ('Artifact Deploy to Nexus') {
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
        
      stage('Application Deployment') {
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
       
 
