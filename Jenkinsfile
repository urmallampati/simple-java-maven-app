pipeline {
parameters {
        choice(choices: "dev\nsit\nstaging\nprod\n", description: 'Environment to deploy', name: 'ENVIRONMENT')
        choice(choices: "create\nupdate\ndelete", description: 'Environment to deploy', name: 'action')
	}
	environment{
            envr="${params.ENVIRONMENT}"
            action=env.getProperty('devint_aws_cred_id')
        }
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            when { anyOf { action 'create'; action 'nupdate' } }
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
