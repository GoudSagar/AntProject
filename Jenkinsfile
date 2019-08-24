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
    }
}
