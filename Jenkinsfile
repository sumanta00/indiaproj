pipeline {
  agent any
  tools {
    jdk 'Java17'
    maven 'Maven'
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
        echo 'Running JUnit tests'
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
        echo 'Building project'
        bat 'mvn clean package -DskipTests'
      }
    }

    stage('Build Docker Image') {
      steps {
        echo 'Building Docker image'
        bat 'docker build -t myjavaproj:1.0 .'
      }
    }

    stage('Run Docker Container') {
      steps {
        echo 'Starting Docker container'
        bat '''
        docker rm -f myjavaproj-container || exit 0
        docker run -d --name myjavaproj-container -p 8080:8080 myjavaproj:1.0
        '''
      }
    }

  }

  post {
    success {
      echo 'Build + Tests + Docker Run SUCCESSFUL!'
    }
    failure {
      echo 'Pipeline FAILED!'
    }
  }

}
