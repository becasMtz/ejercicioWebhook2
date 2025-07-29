pipeline {
    agent any

    environment{
        DOCKER_IMAGE = 'becasmtz/jenkinsfileexample'
    }

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
                //bat 'echo "Building the application"'
                
                /*Si usaramos instrucción docker
                bat 'echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin'
                bat 'docker build -t becasmtz/jenkinsfileexample . && docker push becasmtz/jenkinsfileexample' */

                //Aqui sí se usa "$" en var porque se esta ejecutando directamente en CMND
                bat 'echo %DOCKER_IMAGE%'

                //Si usamos script predefinido de docker
                script{
                    //No se accede a la variable con "$" ya que no se esta ejecutando en el CMD, sino direectamente en ruby porque es parte de la ejecución del pipeline
                    dockerImage = docker.build(DOCKER_IMAGE) 
                }
            }
        }

        stage('Push'){
            steps{
                //Script de plugin de docker que nos deja manejar accesos de manera más simple
                script{
                    docker.withRegistry('https://index.docker.io/v1/','docker-hub-credentials'){
                    // //docker.image(DOCKER_IMAGE).push() -> en caso de no usarse variable
                        dockerImage.push('latest')
                    }
                }
            }

            post{
                success{
                    bat 'echo "Docker image pushed successfully"'
                }
                failure {
                    script {
                    //bat 'echo "Failed to push Docker image"'

                    /*Se accede a una variable local al proceso de jenkins (es accesible cuando se etermina la ejecución de java dentro de jenkins)
                    para obtener el log de lo que hicimos, permitiendo identificar el error e incluirlo en el correo*/

                        def logLines = currentBuild.rawBuild.getLog(100)
                        def logText = logLines.join('\n')
                        mail to: 'rmtzbarron@gmail.com',
                         from: 'rmtzbarron@gmail.com',
                         subject: "Failed to build Docker image ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                         body: "Failed to build Docker image ${env.JOB_NAME} #${env.BUILD_NUMBER} ${env.BUILD_URL} \n\n ${logText}"
                    }
                }
            }
        }
    }

    post{
        success{
            script{
                mail to: 'rmtzbarron@gmail.com',
                from: 'rmtzbarron@gmail.com',
                subject: "Docker image pushed successfully ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Docker image pushed successfully ${env.JOB_NAME} #${env.BUILD_NUMBER}"
            }
        }
    }
}