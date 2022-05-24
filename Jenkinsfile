pipeline{
    environment {
        imagename = "abeerab/frontimage"
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
                        -Dsonar.host.url=http://54.217.252.229:9000/ \
                        -Dsonar.login=admin \
                        -Dsonar.password=admin"
                    } 
                }
            }
        }
        stage("build"){
            
            steps{
                sh '/usr/bin/npm install'
                sh '/usr/bin/npm run build'
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
