pipeline {
  agent any

  parameters {
    choice(name: 'ACTION', choices: ['build', 'deploy', 'remove'], description: 'Pipeline action')
    string(name: 'DOCKER_TAG', defaultValue: 'latest')
  }

  stages {

    stage('Build Image') {
      when { expression { params.ACTION == 'build' } }
      steps {
        sh '''
          docker build -t mydockerhub/spring-app:${DOCKER_TAG} .
          docker login
          docker push mydockerhub/spring-app:${DOCKER_TAG}
          docker rmi mydockerhub/spring-app:${DOCKER_TAG}
          docker logout
        '''
      }
    }

    stage('Deploy') {
      when { expression { params.ACTION == 'deploy' } }
      steps {
        sh '''
          docker-compose pull
          docker-compose up -d
        '''
      }
    }

    stage('Remove') {
      when { expression { params.ACTION == 'remove' } }
      steps {
        sh '''
          docker-compose down
        '''
      }
    }
  }
}
