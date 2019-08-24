pipeline {
    agent any
    tools { 
        maven 'MAVEN' 
        jdk 'JDK_1.8'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
            }
        }

        stage ('Build') {
            steps {
                echo ' is a minimal pipeline.'
                sh 'mvn -v'
                sh 'mvn clean install'
            }
        }
    }
}
