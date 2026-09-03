pipeline {
agent any

```
environment {
    IMAGE_NAME = "student-app"
    VERSION = "1.2"
}

stages {

    stage('Checkout') {
        steps {
            git branch: 'feature/application-change',
                url: 'https://github.com/vishnutg0509/getting-started.git'
        }
    }

    stage('Build') {
        steps {
            sh 'echo "Building Application"'
        }
    }

    stage('Docker Build') {
        steps {
            sh '''
                docker build -t ${IMAGE_NAME}:${VERSION} .
            '''
        }
    }

    stage('Docker Test') {
        steps {
            sh '''
                docker rm -f temp-test || true

                docker run -d \
                    --name temp-test \
                    -p 4000:80 \
                    ${IMAGE_NAME}:${VERSION}

                sleep 15

                curl http://localhost:4000

                docker rm -f temp-test
            '''
        }
    }

    stage('Deploy DEV') {
        steps {
            sh '''
                docker rm -f dev1 || true

                docker run -d \
                    --name dev1 \
                    -p 3001:80 \
                    ${IMAGE_NAME}:${VERSION}
            '''
        }
    }
}

post {
    success {
        echo "Pipeline SUCCESS"
    }

    failure {
        echo "Pipeline FAILED"
    }

    always {
        echo "Pipeline execution completed"
    }
}
```

}
