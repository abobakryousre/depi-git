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
                sh 'docker build . -t abobakryousre/first-app'
            }
        }
        stage("Testing"){
            parallel {
                stage("Unit Testt"){
                    steps {
                        echo "Unit tetst"
                        sh 'sleep 30'
                    }
                }
                stage("Integration Test"){
                    steps {
                        echo "Intgration tetst"
                        sh 'sleep 30'
                    }
                }
                stage("E2E Test"){
                    steps {
                        echo "E2E test"
                        sh 'sleep 30'
                    }
                }
            }
         
        }
        // stage("Release"){
        //     steps {
        //         // docker login username & password
        //         withCredentials([usernamePassword(credentialsId: 'docker', passwordVariable: 'dockerPass', usernameVariable: 'dockerUser')]) {
        //             sh 'echo $dockerPass | docker login -u $dockerUser --password-stdin'
        //             sh 'docker push abobakryousre/first-app'
        //         }
                
        //     }
        // }
        stage("Deploy"){
            // ask for my input -> okay -> 
            steps {
                // input message: 'Continue Deployment to Production?', ok: 'Deploy'
                sh 'docker stop my-app 2> /dev/null || true'
                sh 'docker rm my-app 2> /dev/null || true'
                sh 'docker run --name my-app --rm -d abobakryousre/first-app sleep 1d'
            }
        }
    }
}
