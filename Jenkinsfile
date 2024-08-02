pipeline{
    agent any
    tools{
        nodejs "node"
        jdk "jdk11"
    }
    stages{
        stage("Build"){
            steps{
                sh "npm install"
            }
        }
        stage("Test"){
            steps{
                echo "Testing"
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying"
            }
        }
    }
}