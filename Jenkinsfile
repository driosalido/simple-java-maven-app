pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    triggers {
      GenericTrigger(
            genericVariables: [
                [key: 'ref', value: '$.ref']
            ],
            genericRequestVariables: [
                [key: 'repo'],
            ],
            causeString: 'Triggered on $ref',
	    token: env.KAKA,
     
            printContributedVariables: true,
            printPostContent: true,
    
            regexpFilterText: '$ref;$repo',
            regexpFilterExpression: 'refs/heads/master;maven-test'

    )
  }

    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
    
}
