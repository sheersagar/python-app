pipeline {
    agent{
        label "jenkins-agent" 
    }

    //tools{
    //    maven 'Maven3'
    //    jdk 'Java17'
    //}

    stages{
        stage ("Clean up Workspace") {
            steps{
                git branch: 'main', credentialsId: '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', url: 'https://github.com/sheersagar/python-app.git'
            }
        }

       // stage("Build Application") {
       //     steps{
       //         sh "mvn clean package"
       //     }
       // }

        stage("Test Applicaiton") {
            steps{
                sh "python3 -m unittest discover"
            }
        }


        // For Sonar Qube Analysis

        // stage("Sonar Qube Analysis") {
        //    steps{
        //        script {
        //            withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') {
        //                sh "mv sonar:sonar"
        //            }
        //        }
        //    }   
        //}


        // Build and Push Docker Images

        stage("Build and Push Docker Images") {
            environment {
                DOCKER_IMAGE = "vishv3432/my-custom-pipeline:${BUILD_NUMBER}"
                REGISTRY_CREDENTIALS = credentials('dockerhub')
            }

            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                    dockerImage = docker.image("${DOCKER_IMAGE}")
                    docker.withRegistry('', "${REGISTRY_CREDENTIALS}") {
                        dockerImage.push()
                    }
                 }
            }
        }
    }
}