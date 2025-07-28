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
        }
    }

}