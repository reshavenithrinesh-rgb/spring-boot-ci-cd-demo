pipeline {
    agent any

    stages {
        stage('Environment Check') {
            steps {
                sh '''
                    echo "JAVA_HOME=$JAVA_HOME"
                    which java
                    java -version
                    which javac
                    javac -version
                    mvn -version
                    mvn -q help:evaluate \
                      -Dexpression=maven.compiler.release \
                      -DforceStdout
                '''
            }
        }

        stage('Build and Test') {
            steps {
                sh 'mvn -B clean verify'
            }
        }

        // Keep your existing Docker build and deployment stages below.
    }
}
