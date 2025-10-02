pipeline {
  agent any
  options { skipDefaultCheckout(true); timestamps() }

  environment {
    IMAGE = "<YOUR_DOCKERHUB_USERNAME>/devops-firstproject"  // <-- change this
    TAG   = "build-${env.BUILD_NUMBER}"
  }

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
        sh 'docker build -t $IMAGE:$TAG .'
      }
    }

    stage('Docker Push') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub',
                                          usernameVariable: 'USER',
                                          passwordVariable: 'PASS')]) {
          sh '''
            echo "$PASS" | docker login -u "$USER" --password-stdin
            docker push $IMAGE:$TAG
            docker tag  $IMAGE:$TAG $IMAGE:latest
            docker push $IMAGE:latest
            docker logout
          '''
        }
      }
    }
  }
}
