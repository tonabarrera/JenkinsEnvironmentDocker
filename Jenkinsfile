pipeline {
    agent any
    tools {
        maven 'M3_8_6'
    }
    /*
    stages {
        stage('Dependency Check') {
            steps {
                dir("Curso-Microservicios/"){
                    dependencyCheck additionalArguments: ''' 
                        -o "./" 
                        -s "./"
                        -f "ALL" 
                        --prettyPrint''', odcInstallation: 'dependency_check_7_2_1'
                    dependencyCheckPublisher pattern: 'dependency-check-report.xml'
                }
            }
        }
*/
        stage('Analize') {
            steps {
                dir("Curso-Microservicios/"){
                    withSonarQubeEnv('SonarServer'){
                        sh "mvn clean package sonar:sonar \
                            -Dsonar.projectKey=21_MyCompany_Microservice \
                            -Dsonar.projectName=21_MyCompany_Microservice \
                            -Dsonar.sources=src/main \
                            -Dsonar.coverage.exclusions=**/*TO.java,**/*DO.java,**/example/web/**/*,**/example/persistence/**/*,**/example/commons/**/*,**/example/model/**/* \
                            -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
                            -Djacoco.output=tcpclient \
                            -Djacoco.address=127.0.0.1 \
                            -Djacoco.port=10001"
                    }
                }
            }
        }
/*
        stage('Build') {
            steps {
                dir("Curso-Microservicios/"){
                    sh "docker build -t microservicio ."
                }
            }
        }

         stage('Push Image') {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker_nexus', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                    sh "docker login -u $USERNAME -p $PASSWORD 192.168.100.5:8083"
                    sh "docker tag microservicio:latest 192.168.100.5:8083/repository/docker-private/microservicio:latest"
                    sh "docker push 192.168.100.5:8083/repository/docker-private/microservicio:latest"
                }
            }
        }
*/
        stage('Liquibase') {
            steps {
                dir("liquibase/"){
                    sh '/opt/liquibase/liquibase --changeLogFile="changesets/db.changelog-master.xml" update'
                }
            }
        }
        
    }
}