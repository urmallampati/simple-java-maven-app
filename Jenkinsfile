pipeline {
parameters {
        choice(choices: "dev\nsit\nstaging\nprod\n", description: 'Environment to deploy', name: 'ENVIRONMENT')
        choice(choices: "create\nupdate\ndelete", description: 'Environment to deploy', name: 'action')
	}

    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            when {  expression { params.action == 'create'  || params.action == 'update'}}
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
    }
}
