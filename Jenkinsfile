import java.text.SimpleDateFormat

pipeline {
    agent {
        label "test"
    }
    environment{
        PROJECT="tools"
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
                sh "docker image build --no-cache -t softtekcoe/${env.PROJECT} ${env.PROJECT}"
                sh "docker image tag softtekcoe/${env.PROJECT} softtekcoe/${env.PROJECT}:${currentBuild.displayName}"
            }
        }
        stage("publish"){
            steps {
                sh "docker image push softtekcoe/${env.PROJECT}:${currentBuild.displayName}"
            }
        }
        stage("publish latest"){
            steps {
                sh "docker image push softtekcoe/${env.PROJECT}"
            }
        }
    }
    post {
        always {
            sh "docker system prune -f"
        }
    }
}
