pipeline {
    agent any

    triggers{
        githubPush()
    }

    stages{
        stage('Checkout'){
            steps{
                git branch: 'master', url: 'https://github.com/becasMtz/ejercicioWebhook2'
            }
        }

        stage('Build'){
            steps{
                bat 'echo "Building the application"'
            }
        }
    }

}