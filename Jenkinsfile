pipeline {
    agent any

     tools {
     
        maven 'maven'

    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        
    //    stage('SonarQube Analysis') {
      //      steps {
         //       withSonarQubeEnv('SonarQube') {
           //         sh 'mvn sonar:sonar'
             //   }
            //}
        //}
        
        stage('Publish to Artifactory') {
            steps {
                rtMavenDeployer(
                    id: 'Jfrog', 
                    serverId: 'Jfrog', 
                    releaseRepo: 'example-repo-local', 
                    snapshotRepo: 'example-repo-local'
                )
            }
        }
        stage('Code Coverage and SonarQube Analysis') {
            steps {
                // Set up Jacoco for code coverage
                sh 'mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent test'
                
                // Generate code coverage report
                sh 'mvn org.jacoco:jacoco-maven-plugin:report'
                
                // Start SOnarQube analysis and publish code coverage results to SonarQube
                withSonarQubeEnv('SonarQube') {
                    sh "mvn sonar:sonar -Dsonar.projectKey=JacocoExample-new-3 -Dsonar.projectName='JacocoExample-new-3'"
        }
               
}
}
        stage('Upload'){
            steps{
                rtUpload (
                 serverId:"Jfrog",
                  spec: '''{
                   "files": [
                      {
                      "pattern": "*.war",
                      "target": "example-repo-local/"
                      }
                            ]
                           }''',
                        )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "Jfrog"
                )
            }
        }
        
   // stage("Quality gate") {
     //     steps {
       //     waitForQualityGate abortPipeline: true
          
        
        
     //}
    //}
   //    post {
//        always {
            // Configure the SonarQube Quality Gate
  //          script {
    //            def qg = waitForQualityGate()
      //          if (qg.status != 'OK') {
        //            error "Pipeline failed due to Quality Gate status: ${qg.status}"
          //      }
            //}
       // }
    //}
}
}
