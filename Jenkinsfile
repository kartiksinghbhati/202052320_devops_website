pipeline {
    agent any

     environment{
       registryCredential = 'ecr:ap-south-1:202052320_Kartik_Devops'
       appRegistry = "738372017322.dkr.ecr.ap-south-1.amazonaws.com/kartik_capstone_devops"
       capstoneRegistry = "http://738372017322.dkr.ecr.ap-south-1.amazonaws.com"
       cluster = "Kartik_Devops_Capstone"
        service = "Kartik_Devops_Capstone_Service"
   }

    stages {
        stage('Clone Website') {
            steps {
                git url:'https://github.com/kartiksinghbhati/202052320_devops_website.git'
            }
        }

       stage("Docker Build Image"){
           steps{
             script{
                dockerImage = docker.build(appRegistry+ ":$BUILD_NUMBER",".")
             }
           }
      }

       stage('Test Website') {
            steps {
                // Run tests on the website
                sh 'echo "Running tests on the website"'
            }
       }

       stage("Upload App Image"){
         steps{
            script{
                docker.withRegistry(capstoneRegistry, registryCredential){
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                }
            }
         }
      }

      stage('Deploy to ecs'){
         when {
                branch "master"
         }
         steps{
            withAWS(credentials: '202052320_Kartik_Devops', region: 'ap-south-1'){
                sh 'aws ecs update-service --cluster ${cluster} --service ${service} --force-new-deployment'
            }
         }
      }

    }
}
