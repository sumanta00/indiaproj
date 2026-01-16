pipeline {
  agent any

  tools {
    jdk 'JDK21'
    maven 'Maven-3.9.12'
  }

  stages {

    stage('Checkout Code') {
      steps {
        echo 'Pulling from GitHub'
        git branch: 'main', url: 'https://github.com/sumanta00/indiaproj.git'
      }
    }

    stage('Test Code') {
      steps {
        echo 'Running JUnit Tests'
        bat 'mvn clean test'
      }
      post {
        always {
          junit '**/target/surefire-reports/*.xml'
        }
      }
    }

    stage('Build Project') {
      steps {
        echo 'Building JAR'
        bat 'mvn clean package -DskipTests'
      }
    }

    stage('Build Docker Image') {
      steps {
        echo 'Building Docker Image'
        bat 'docker build -t indiaproj:1.0 .'
      }
    }

    stage('Run Docker Container') {
      steps {
        echo 'Starting Docker Container'
        bat '''
        docker rm -f indiaproj-container || exit 0
        docker run -d --name indiaproj-container -p 8080:8080 indiaproj:1.0
        '''
      }
    }

  }

  post {
    success {
      echo 'Build + Docker Run SUCCESS!'
    }
    failure {
      echo 'Pipeline FAILED!'
    }
  }
}
