pipeline {
    agent any
    environment {
        ROOT_PATH  =  "C:/Program Files/Apache Software Foundation/Tomcat 10.1"
        APP_PATH   =  "$ROOT_PATH/webapps"
        TEMP_DIR   =  "${env.WORKSPACE}/web-thymeleaf-war"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Compile..'
                echo "workspace ${env.WORKSPACE}" 
                echo "BuildNumber :: ${env.BUILD_NUMBER}"
                
                echo "generating war file"
                dir('web-thymeleaf-war') {
                    // Clean and build using Maven
                    bat "mvn clean package"
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo "Stop catalina"
                sh '''

                  cd ${ROOT_PATH}/bin/
                  pwd
                  catalina.bat stop

                '''

                echo 'Deploying....'
                echo "Remove older war files"
                sh '''
                   ls -ltr $APP_PATH
                   rm -rf $APP_PATH/sample*

                   echo "After removing older application (sample)"
                   ls -ltr $APP_PATH/
                   
                   echo "deploy new application"
                '''
                sh "ls -ltr ${TEMP_DIR}/sample_${env.BUILD_NUMBER}.war"
                sh "cp ${TEMP_DIR}/sample_${env.BUILD_NUMBER}.war $APP_PATH/sample.war"
                sh "ls -ltr $APP_PATH" 
            }
        }
       stage('Start Catalina') {
           steps {
               echo "Start catalina"
               echo "pwd"
                sh '''

                  cd $ROOT_PATH
                  pwd
                  nohup bin/catalina.bat start &

                '''
           }
       }
    }
}
