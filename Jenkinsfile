pipeline {
    agent any

    enviroment{
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
                
                //Si usamos script predefinido de docker
                script{
                    //Aqui sí se usa "$" en var porque se esta ejecutando directamente en CMND
                    bat 'echo $DOCKER_IMAGE'
                    //No se accede a la variable con "$" ya que no se esta ejecutando en el CMD, sino direectamente en ruby porque es parte de la ejecución del pipeline
                    docker.build(DOCKER_IMAGE) 
                }
            }
        }
    }

}