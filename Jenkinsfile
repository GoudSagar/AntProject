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
                echo ' is a minimal pipeline.'
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
       
       stage ('Sonar')
             environment {
             scannerHome = tool 'SONAR_SCANNER'
             }
             steps {
                echo 'Running Static Code Analysis'
                 withSonarQubeEnv('sonarqube') {
                 sh "$(scannerHome}/bin/sonar-scanner"
                 }
                 timeout(time: 10, unit: 'MINUTES') {
                 waitForQualityGate abortPipeline: true
                   }
            }
      }
   }  
}

