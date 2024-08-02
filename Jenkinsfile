pipeline{
    environment{
     NodejsHome= tool "myNode"
     PATH = "${NodejsHome}/bin:${PATH}"   
    }
    agent any
  
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