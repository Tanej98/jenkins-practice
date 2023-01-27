// template file
pipeline {
    
    agent any
    
    environment {
        NEW_VERSION = '1.3.0'
        SERVER_CREDENTIALS = credentials('id-of-credenials') //need to install aditional package
    }
    
    //tools used in software like gradle, maven and jdk
    // for other we need to use wrapper inside the stages
    tools {
    }
    
    parameters {
        string(name: 'VERSION', defaultValue: '', description: 'version to deploy on prod')
        choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: '')
        booleanParam(name: 'executeTests', defaultValue: True, description:'')
        
    }
    
    stages {
        
        stage("build") {
            
            // conditions
             when {
                 expression {
                     BRANCH_NAME == 'dev' && CODE_CHANGES == true
                 }
             }
            
            steps {
                
                script {
                    // can write code in groovy script
                    gv = load "script.groovy" //  create script.groovy
                    
                    gv.buildApp() // call functions from the script
                }
                
                echo 'Building the application...' 
                echo "VESION ${NEW_VERSION}"
            }
        }
        
         stage("test") {
            
             // conditions
             when {
                 expression {
                     BRANCH_NAME == 'dev' || BRANCH_NAME == 'master' && params.executeTests == true
                 }
             }
             
            steps {
                echo "Testing the application..."    
            }
        }
        
         stage("deploy") {
            
            steps {
                echo "Deploying the application..." 
                
                withCredentials([
                    usernamePassword(credentials: 'id-of-credentials', usernameVariable: USER, passwordVariable: PWD)
                ]){
                    sh "some script ${USER} ${PWD}"
                }
            }
        }
    }
    
    // after all stages are completed
    post {
        
        always {
            
        }
        
        success {
            
        }
        
        failure {
            
        }
    }
}
