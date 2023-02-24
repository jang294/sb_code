pipeline {
    agent any
    // any, none, label, node, docker, dockerfile, kubernetes
    tools {
        maven 'my_maven'
    }

    environmnet {
        gitName = 'jongsamsunduck'
        gitEmail = 'tjrwjsjje@naver.com'
        gitWebaddress = 'https://github.com/jongsamsunduck/sb_code.git'
        gitSshaddress = 'git@github.com:jongsamsunduck/sb_code.git'
        gitCredential = 'git_cre' //github credential 생성시 ID
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
    }
}
