pipeline{
    agent any
    stages {
        stage ('Build Backend'){
            steps {
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Tests'){
            steps {
                bat 'mvn test'
            }
        }
        stage ('Sonar Analysis'){
            environment{
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL'){
                    bat "cd ${scannerHome}"
                    bat "echo /bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=180cc27e356e3e61334836b950d86db63afeddfd -Dsonar.java.binaries=target -Dsonar.coverage.exclusions=**/mvn/**,**/src/test/**,**/model/**,**Application.java"
                }
            }    
        }
        stage ('Quality Gate'){
            steps{
                timeout(time: 1, unit: 'MINUTES'){
                    withForQualityGate abortPipeline: true
                } 
            }
        }
    }
}


