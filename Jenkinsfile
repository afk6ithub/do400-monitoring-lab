pipeline {
    agent { node { label 'maven' } }

    stages {

        stage('Test') {
            steps {
                sh './mvnw clean test'
            }
        }

        stage('Build') {
            environment {
                QUAY = credentials('MONITORING_LAB_QUAY_CREDENTIALS')
            }
            steps {
                // Add Quarkus container extensions
                sh './scripts/include-container-extensions.sh'

                // Package app, build image, and push to Quay
                sh '''
                ./scripts/build-and-push-image.sh \
                  -b $BUILD_NUMBER \
                  -u "$QUAY_USR" \
                  -p "$QUAY_PSW" \
                  -r do400-monitoring-lab
                '''
            }
        }

    }
}
