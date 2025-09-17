pipeline {
    agent any
    
    environment {
        GIT_BRANCH   = 'main'
        GIT_URL      = 'git@github.com:pichuka123/mindcircuit16d_java_war_project.git'
        MAVEN_CMD    = 'mvn clean install'
        TOMCAT_URL   = 'http://ec2-3-91-76-30.compute-1.amazonaws.com:8081/'
        TOMCAT_CRED  = 'tomcat'
        CONTEXT_PATH = 'my-pipeline-app'
        WAR_FILE     = '**/*.war'
    }
    
    stages {
    
        stage('CLONE SCM') {
            steps {
                echo "Job: ${JOB_NAME}, Build Number: ${BUILD_NUMBER}, Workspace: ${WORKSPACE}"
                echo "Cloning from ${GIT_URL}, branch: ${GIT_BRANCH}"
                
                git branch: "${GIT_BRANCH}", url: "${GIT_URL}"
            }
        }
		
        stage('Build Artifact') {
            steps {
                echo "Build ID: ${BUILD_ID}, Executing Maven in workspace ${WORKSPACE}"
                sh "${MAVEN_CMD}"
            }
        }
		
        stage('Deploy to Tomcat') {
            steps {
                echo "Deploying ${WAR_FILE} from job ${JOB_NAME} build #${BUILD_NUMBER}"
                echo "Target Tomcat: ${TOMCAT_URL}, Context Path: ${CONTEXT_PATH}"
                
                deploy adapters: [tomcat9(
                    alternativeDeploymentContext: '', 
                    credentialsId: "${TOMCAT_CRED}", 
                    path: '', 
                    url: "${TOMCAT_URL}"
                )], contextPath: "${CONTEXT_PATH}", war: "${WAR_FILE}"
            }
        }		
    }
    
    post {
        success {
            echo "✅ Build ${BUILD_NUMBER} of job ${JOB_NAME} completed successfully!"
        }
        failure {
            echo "❌ Build ${BUILD_NUMBER} of job ${JOB_NAME} failed. Check console logs in ${BUILD_URL}"
        }
    }
}
