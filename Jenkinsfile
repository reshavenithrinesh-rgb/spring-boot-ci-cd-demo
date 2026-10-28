
pipeline {
    agent any

    stages {
        stage('Build and Test') {
            steps {
                sh 'mvn -B clean verify'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t spring-boot-ci-cd-demo .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f spring-boot-ci-cd-demo || true

                    docker run -d \
                      --restart unless-stopped \
                      --name spring-boot-ci-cd-demo \
                      -p 8081:8080 \
                      spring-boot-ci-cd-demo
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    for i in $(seq 1 20); do
                        if curl -fsS http://localhost:8081/api/hello; then
                            echo "Application is running successfully!"
                            exit 0
                        fi
                        sleep 3
                    done

                    echo "Application did not respond in time."
                    docker logs spring-boot-ci-cd-demo
                    exit 1
                '''
            }
        }
    }
}

