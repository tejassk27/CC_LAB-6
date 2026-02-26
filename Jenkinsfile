pipeline {
    agent any
    stages {
    stage('Build Backend Image') {
    steps {
        sh '''
        docker rmi -f backend-app || true
        # Removed "CC_LAB-6/" prefix from the path
        docker build -t backend-app backend
        '''
    }
}
stage('Deploy NGINX Load Balancer') {
    steps {
        sh '''
        docker rm -f nginx-lb || true
        docker run -d --name nginx-lb --network app-network -p 80:80 nginx
        # Removed "CC_LAB-6/" prefix here too
        docker cp nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf
        docker exec nginx-lb nginx -s reload
        '''
    }
}
        stage('Deploy NGINX Load Balancer') {
            steps {
                sh '''
                docker rm -f nginx-lb || true
                
                docker run -d \
                  --name nginx-lb \
                  --network app-network \
                  -p 80:80 \
                  nginx
                
                docker cp CC_LAB-6/nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf
                docker exec nginx-lb nginx -s reload
                '''
            }
        }
    }
    post {
        success {
            echo 'Pipeline executed successfully. NGINX load balancer is running.'
        }
        failure {
            echo 'Pipeline failed. Check console logs for errors.'
        }
    }
}
