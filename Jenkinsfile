pipeline {
  agent any
  options { skipDefaultCheckout(true); timestamps() }
  stages {
    stage('git') {
      steps {
        git branch: 'Ibtihel-Mnaja',
            url: 'https://github.com/miriambenouaghrem/devops_firstProject.git'
      }
    }
    stage('Build, Test & Package') {
      steps {
        sh '''
          chmod +x mvnw
          ./mvnw -B clean package -DskipTests
        '''
      }
    }
    stage('Docker Build') {
      steps {
        sh 'docker build -t ibtihel/devops-firstproject:build-${BUILD_NUMBER} .'
      }
    }
    environment {
  IMAGE = "<your-dockerhub-username>/devops-firstproject"
  TAG   = "build-${env.BUILD_NUMBER}"
}

stage('Docker Push') {
  steps {
    // tag also as 'latest' (handy for pulls)
    sh '''
      docker tag $IMAGE:$TAG $IMAGE:latest || true
    '''
    withCredentials([usernamePassword(credentialsId: 'dockerhub',
                                      usernameVariable: 'USER',
                                      passwordVariable: 'PASS')]) {
      sh '''
        echo "$PASS" | docker login -u "$USER" --password-stdin
        docker push $IMAGE:$TAG
        docker push $IMAGE:latest
        docker logout
      '''
    }
  }
}
  }
}
