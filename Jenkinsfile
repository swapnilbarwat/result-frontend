def version = ''
node {
   stage('checkout') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/swapnilbarwat/result-frontend.git'
      version = readFile('version').trim()
      currentBuild.displayName = "${version}-${env.BRANCH_NAME}"
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
   }
   stage('Build') {
       docker.withRegistry('https://docker.io', 'docker-hub-credentials') {
          def app = docker.build("harshals/result-frontend:${version}")
          sh "docker push docker.io/harshals/result-frontend:${version}"
       }
       withEnv(["VAMP_HOST=http://104.197.251.15:8080"]) {
          sh "/var/lib/jenkins/vamp/vamp-cli-0.7.9/bin/vamp create breed --file breeds/postgres.yml"
          sh "/var/lib/jenkins/vamp/vamp-cli-0.7.9/bin/vamp create breed --file breeds/result.yml"
       }
    }
}

node {
   stage('50-50% deployment') { // for display purposes
      input message: 'Deploy to cluster? This will rollout new build to 50% cluster.'
   }
}

node {
   stage('move full') { // for display purposes
      input message: 'Deploy to full cluster?'
   }
   stage('undeploy previous version') {
      echo "undeploying cluster"
   }
}