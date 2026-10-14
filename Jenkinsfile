pipeline {
    agent any

    stages {
        stage("Checkout") {
            steps {
                echo "checkout"
                git branch: 'main', credentialsId: 'gitUser', url: 'https://github.com/abobakryousre/depi-git.git'
                sh 'ls'
            }
        }
        stage("Build"){
            steps {
                sh "docker build . -t abobakryousre/first-app:$BUILD_NUMBER"
            }
        }
        stage("Testing"){
            parallel {
                stage("Unit Testt"){
                    steps {
                        echo "Unit tetst"
                        sh 'sleep 3'
                    }
                }
                stage("Integration Test"){
                    steps {
                        echo "Intgration tetst"
                        sh 'sleep 5'
                    }
                }
                stage("E2E Test"){
                    stages {
                        stage("Test Backend") {
                            steps    {
                                echo "testing backend"
                                sh 'sleep 2'
                            }
                        }
                        stage("Test Database"){
                            steps {
                                echo "testing database"
                                sh 'sleep 2'
                                
                            }
                        }
                    }
                }
            }
         
        }
        stage("Release"){
            steps {
                // docker login username & password
                withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'dockerPass', usernameVariable: 'dockerUser')]) {
                    sh 'echo $dockerPass | docker login -u $dockerUser --password-stdin'
                    sh "docker push abobakryousre/first-app:$BUILD_NUMBER"
                }
                
            }
        }
        stage("Deploy"){
            // ask for my input -> okay -> 
            steps {
                // input message: 'Continue Deployment to Production?', ok: 'Deploy'
                sh 'docker stop my-app 2> /dev/null || true'
                sh 'docker rm my-app 2> /dev/null || true'
                sh "docker run --name my-app --rm -d abobakryousre/first-app:$BUILD_NUMBER sleep 1d"
            }
        }
    }
}
