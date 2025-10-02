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
  }
}
