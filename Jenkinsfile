pipeline {

    agent any
    environment {
        YOUR_NAME = credentials("YOUR_NAME")
    }
    stages {

        stage('Build') {

            steps {

                sh '''
                docker build -t odavies15/task2-db db
                docker build -t odavies15/task2-app flask-app
                docker build -t odavies15/task2-nginx nginx
                '''

            }

        }

        stage('Push') {

            steps {

                sh '''
                docker push odavies15/task2-db
                docker push odavies15/task2-app
                docker push odavies15/task2-nginx
                '''

            }

        }

        stage('Deploy') {

            steps {

                sh '''
                ssh jenkins@oli-deploy <<EOF
                docker stop nginx && echo "Stopped nginx" || echo "nginx is not running"
                docker rm nginx && echo "removed nginx" || echo "nginx does not exist"
                docker stop flask-app && echo "Stopped flask-app" || echo "flask-app is not running"
                docker rm flask-app && echo "removed flask-app" || echo "flask-app does not exist"
                docker stop db && echo "Stopped db" || echo "db is not running"
                docker rm db && echo "removed db" || echo "db does not exist"
                docker network rm task2-net && echo "removed network" || echo "network already removed"
                docker network create task2-net
                docker run -d --name db --network task2-net -p 80:80 odavies15/task2-db
                docker run -d --name flask-app --network task2-net odavies15/task2-app
                docker run -d --name nginx --network task2-net -p 80:80 odavies15/task2-nginx
                '''

            }

        }

    }

}