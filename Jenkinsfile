pipeline {

    agent any
    environment {
        MYSQL_ROOT_PASSWORD = credentials("MYSQL_ROOT_PASSWORD")
    }
    stages {

        stage('Build') {

            steps {

                sh '''
                docker build -t odavies15/task2-db db
                docker build -t odavies15/task2-app flask-app
                
                '''

            }

        }

        stage('Push') {

            steps {

                sh '''
                docker push odavies15/task2-db
                docker push odavies15/task2-app
                
                '''

            }

        }

        stage('Deploy') {

            steps {

                sh '''
                sed -e 's,{{password}},'${MYSQL_ROOT_PASSWORD}',g;' db-password.yaml | kubectl apply -f -
                kubectl apply -f db-manifest.yaml
                kubectl apply -f app-manifest.yaml
                kubectl apply -f nginx-config.yaml
                kubectl apply nginx-pod.yaml
                sleep 60
                kubectl get services
                '''

            }

        }

    }

}