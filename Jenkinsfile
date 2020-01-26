@Library('my-shared-library') _
import com.mycompany.app.App
pipeline {
parameters {
        choice(choices: "dev\nsit\nstaging\nprod\n", description: 'Environment to deploy', name: 'ENVIRONMENT')
        booleanParam(defaultValue: false, description: 'Upload configuration files to S3', name: 'uploadConfig')
        booleanParam(defaultValue: false, description: 'Upload scripts to S3', name: 'uploadStripts')
        booleanParam(defaultValue: false, description: 'dummy param', name: 'dummyParam')
                booleanParam(defaultValue: false, description: 'dummy param', name: 'dummyParam2')

	}
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}
