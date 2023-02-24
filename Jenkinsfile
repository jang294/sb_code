pipeline {
    agent any
    // any, none, label, node, docker, dockerfile, kubernetes
    tools {
        maven 'my_maven'
    }

    environment {
        gitName = 'jongsamsunduck'
        gitEmail = 'tjrwjsjje@naver.com'
        gitWebaddress = 'https://github.com/jongsamsunduck/sb_code.git'
        gitSshaddress = 'git@github.com:jongsamsunduck/sb_code.git'
        gitCredential = 'git_cre' //github credential 생성시 ID
        dockerHubRegistry = 'jang1023/sbimage'
        dockerHubRegistryCredential = 'docker_cre' //docker credential 생성시 ID
    }

    stages {
        stage('Checkout Github') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: gitCredential, url: gitWebaddress]]])
            }
            post {
                failure {
                    echo 'Reppository clone failure'
                }
                success {
                    echo 'Reppository clone success'
                }
            }
        }
        stage('Maven Build') {
         steps {
            sh 'mvn clean install'
            //maven 플러그인이 미리 설치되어 있어야함
         }
            post {
                failure {
                    echo 'Reppository clone failure'
                }
                success {
                    echo 'Reppository clone success'
                }
            }
        }
        stage('Docker image Build') {
         steps {
            sh "docker build -t ${dockerHubRegistry}:${currentBuild.number} ."
            sh "docker build -t ${dockerHubRegistry}:latest ."
            //jang1023/sbimage:x 이런 식으로 빌드가 될 것이다.
            //currentBuild.number 젠킨스에서 제공하는 빌드 수
         }
            post {
                failure {
                    echo 'Reppository clone failure'
                }
                success {
                    echo 'Reppository clone success'
                }
            }
        }
        stage('docker image push') {
          steps {
            withDockerRegistry(credentialsId: dockerHubRegistryCredential, url: '') {
          // withDockerRegistry : docker pipeline 플러그인 설치시 사용가능.
          // dockerHubRegistryCredential : environment에서 선언한 docker_cre  
            sh "docker push ${dockerHubRegistry}:${currentBuild.number}"
            sh "docker push ${dockerHubRegistry}:latest"
            }

         }
            post {
                failure {
                    echo 'Reppository clone failure'
                }
                success {
                    echo 'Reppository clone success'
                }
            }
        }
        stage('docker container deploy') {
          steps {
            sh "docker rm -f sb"
            sh "docker run -dp 5656:8085 --name sb ${dockerHubRegistry}:${currentBuild.number}"
            }

            post {
                failure {
                    echo 'failure'
                }
                success {
                    echo 'success'
                }
            }
        }
    }
}
