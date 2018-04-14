import java.text.SimpleDateFormat

pipeline {
    agent {
        label "test"
    }
    options {
        disableConcurrentBuilds()
    }
    stages {
        stage("init") {
            steps {
                script {
                    def dateFormat = new SimpleDateFormat("yy.MM.dd")
                    currentBuild.displayName = dateFormat.format(new Date()) + "-" + env.BUILD_NUMBER
                }
                withCredentials([usernamePassword(
                            credentialsId: "docker",
                            usernameVariable: "USER",
                            passwordVariable: "PASS"
                    )]) {
                        sh "docker login -u $USER -p '${PASS}'"
                    }
            }
        }
        stage("build"){
            steps {
                sh "docker image build --no-cache -t softtekcoe/zsh zsh"
                sh "docker image tag softtekcoe/zsh softtekcoe/zsh:${currentBuild.displayName}"
            }
        }
        stage("publish"){
            steps {
                sh "docker image push softtekcoe/zsh:${currentBuild.displayName}"
            }
        }
        stage("publish latest"){
            when{
                branch "master"
            }
            steps {
                sh "docker image push softtekcoe/zsh"
            }
        }
    }
    post {
        always {
            sh "docker system prune -f"
        }
    }
}
