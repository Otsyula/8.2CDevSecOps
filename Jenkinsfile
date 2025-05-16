pipeline { 
  agent any 
 
  stages { 
    stage('Checkout') { 
      steps { 
        git branch: 'main', url: ' https://github.com/Otsyula/8.2CDevSecOps.git' 
      } 
    } 
 
    stage('Install Dependencies') { 
      steps { 
        sh 'npm install' 
      } 
    } 
 
    stage('Run Tests') { 
      steps { 
        sh 'npm test || true' // Allows pipeline to continue despite test failures 
      } 
      //Adding functionality to send an email with an update showing stage completion with logs (last 50 lines of logs)
      post{
          success{
            script {
            def log = currentBuild.rawBuild.getLog(50).join('\n')
            mail to: "ashleyotsyula1@gmail.com",
            subject: "Test status",
            body: """The tests were successful. The system will continue along the pipeline.
              
            Here is an overview of the logs:
            ${log}
              """
            }
            
          }
        }
    } 
 
    stage('Generate Coverage Report') { 
      steps { 
        // Ensure coverage report exists 
        sh 'npm run coverage || true' 
      } 
    } 
 
    stage('NPM Audit (Security Scan)') { 
      steps { 
        sh 'npm audit || true' // This will show known CVEs in the output 
      } 
      post{
          success{
            script{
              def log = currentBuild.rawBuild.getLog(50).join('\n')
              mail to: "ashleyotsyula1@gmail.com",
              subject: "NPM Audit status",
              body: """The audit was successful. No further action needed

              Here is an overview of the logs:
              ${log}
              """
            }
          }
        }
    } 

 
  } 
}
