pipeline {
    agent any

    stages {

        stage("Clone") {
            steps {
                git url: "https://github.com/lokesh276/Python-CI-CD.git",
                    branch: "loki-dev"
            }
        }
        
        stage("Scan the code"){
            steps{
                sh "trivy fs . -o result.json"
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }

        stage("Test") {
            steps {
                echo "Written by tester or developer"
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: "dockerHubCreds",
                        usernameVariable: "dockerUser",
                        passwordVariable: "dockerPass"
                    )
                ]) {
                    sh "docker login -u ${dockerUser} -p ${dockerPass}"
                    sh "docker tag two-tier-flask-app ${dockerUser}/two-tier-flask-app"
                    sh "docker push ${dockerUser}/two-tier-flask-app"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose up -d --build"
            }
        }
    }

    post {
        success {
            emailext(
                subject: "Build Successful",
                body: "Good news Your Jenkins build was successful!",
                to: "lokeshthakur1500@gmail.com"
            )
        }

        failure {
            emailext(
                subject: "Build Failed",
                body: "Bad news Your Jenkins build has failed. Please check logs.",
                to: "lokeshthakur1500@gmail.com"
            )
        }
    }
}
