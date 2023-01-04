pipeline {
  agent any
  stages {
    stage('Compile') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''mvn clean verify sonar:sonar \\
  -Dsonar.projectKey=Petclinic \\
  -Dsonar.host.url=http://172.31.30.140:9000 \\
  -Dsonar.login=sqp_5e69256ce8bf39bdf4d3eccb2a660c6050fb24af'''
      }
    }

  }
}