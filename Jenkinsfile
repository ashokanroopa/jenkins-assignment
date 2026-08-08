pipeline {

    agent any

    parameters {

        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'prod'],
            description: 'Select Environment'
        )

        string(
            name: 'VERSION',
            defaultValue: '1.0',
            description: 'Application Version'
        )

    }

    stages {

        stage('Checkout') {

            steps {

                echo "Checking out Source Code"

                checkout scm

            }

        }

        stage('Build') {

            steps {

                echo "Building Version ${params.VERSION}"

                sh 'mvn clean package'

            }

        }

        stage('Test') {

            steps {

                echo "Running Tests"

                sh 'mvn test'

            }

        }

        stage('Deploy') {

            steps {

                echo "Deploying Version ${params.VERSION}"

                echo "Environment : ${params.ENVIRONMENT}"

                sh 'echo Deploying Application...'

            }

        }

    }

    post {

        success {

            archiveArtifacts artifacts: 'target/*.jar'

            build job: 'Deploy-Job'

        }

        always {

            script {

                currentBuild.description =
                "Version=${params.VERSION} Environment=${params.ENVIRONMENT}"

            }

        }

    }

}
