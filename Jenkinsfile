pipeline {
  agent {
    dockerfile {
      dir 'jenkins'
      args '--volume /var/run/docker.sock:/var/run/docker.sock'
      label 'docker'
    }
  }

  environment {
  }
  
  parameters {
    string(
      name: 'CON_REPO',
      description: 'path to container repo'
      defaultVaule: 'localhost'
    )

  stages {

    stage('Install Deps') {
      steps {
        echo "Installing Deps..."
	npm install
      }
    }
    stage('Unit Test')
      steps {
        echo 'Testing..'
        sh """
        npm run test
        """
      }
    }

    stage('Containerize')
      steps {
        echo 'Containerizing..'
        sh """
        cd ./client
        docker build -t ${CON_REPO}/bridgette-core:${params.BRANCH} .
        """
      }
    }
    stage('Deploy Containers'){
      steps{
	docker push ${CON_REPO}/bridgette-core:${params.BRANCH}
	}
    }
}
