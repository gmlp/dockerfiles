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
                sh "docker image build --no-cache -t softtekcoe/jenkins jenkins"
                sh "docker image tag softtekcoe/jenkins softtekcoe/jenkins:${currentBuild.displayName}"
                sh "docker image build --no-cache -t softtekcoe/jenkins-agent jenkins-agent"
                sh "docker image tag softtekcoe/jenkins-agent softtekcoe/jenkins-agent:${currentBuild.displayName}"
            }
        }
        stage("publish"){
            steps {
                sh "docker image push softtekcoe/jenkins:${currentBuild.displayName}"
                sh "docker image push softtekcoe/jenkins-agent:${currentBuild.displayName}"
            }
        }
        stage("publish latest"){
            steps {
                sh "docker image push softtekcoe/jenkins"
                sh "docker image push softtekcoe/jenkins-agent"
            }
        }
    }
    post {
        always {
            sh "docker system prune -f"
        }
    }
}
