pipeline{
    environment {
        imagename = "saifromdhane/solarenergy_front"
        registryCredential = "dockerhub_credentials"
        // dockerImage = ''
        def scannerHome = tool 'sonarqube-scanner'
    }
    agent any
    stages{
        stage("test-sonar"){
            steps{
                script {
                    withSonarQubeEnv("sonarQube") {
                    sh "${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=solarenergy-frontend \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=admin \
                        -Dsonar.password=trythisagain96"
                    } 
                }
            }
        }
        stage("build"){
            
            steps{
                sh 'npm install'
                sh 'npm run build'
            }
        }
        
        stage("docker-build"){
            steps{
                script {
                    dockerImage = docker.build imagename   
                    docker.withRegistry( '', registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                    }
                }
            }
        }
        stage("deploy"){
            steps{
                echo 'deployment'
            }
        }
    }
}
