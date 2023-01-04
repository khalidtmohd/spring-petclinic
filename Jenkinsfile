pipeline {
  agent {
    node {
      label 'test'
    }

  }
  stages {
    stage('Compile') {
      steps {
        sh './mvnw clean compile'
      }
    }

    stage('Static Analysis') {
      steps {
        sh '''./mvnw  sonar:sonar \\
  -Dsonar.projectKey=Petclinic \\
  -Dsonar.host.url=http://172.31.30.140:9000 \\
  -Dsonar.login=sqp_5e69256ce8bf39bdf4d3eccb2a660c6050fb24af'''
      }
    }

    stage('Unit test') {
      steps {
        sh './mvnw "-Dtest=**/petclinic/*/*.java" test'
      }
    }

    stage('Package') {
      steps {
        sh './mvnw package -DskipTests=true'
      }
    }

    stage('Deploy') {
      parallel {
        stage('QA Deploy') {
          steps {
            sh './mvnw spring-boot:run </dev/null &>/dev/null &'
            junit '**/target/surefire-reports/'
            perfReport '**/target/jmeter/results/*'
          }
        }

        stage('Integration & Performance tests') {
          steps {
            sh './mvnw verify'
          }
        }

      }
    }

  }
}