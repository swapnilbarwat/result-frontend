def version = ''
node {
   stage('checkout') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/swapnilbarwat/voting-frontend.git'
      version = readFile('version').trim()
      currentBuild.displayName = "${version}-${env.BRANCH_NAME}"
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
   }
   stage('Build') {
       docker.withRegistry('http://localhost:5000') {
          def app = docker.build "voting-frontend:${version}"
          app.push("${version}")
       }
    }
}
node {
   stage('50-50% deployment') { // for display purposes
   input(message: 'Deploy to cluster?', ok: 'Yes', 
         parameters: [booleanParam(defaultValue: true, 
         description: 'Yes will deploy to 50% of cluster',name: 'Yes?')])
  }
}
stage "Move full"
input message: 'Deploy to full cluster?', submitter: 'Yes'
node {
   stage('Move full') { // for display purposes
     echo "This will checkout blueprint yaml and deploy to production"
   }
   stage('undeploy previous version') { // for display purposes
     echo "This will checkout blueprint yaml and deploy to production"
   }
}