// Jenkinsfile (Declarative Pipeline)
pipeline {
    // 1. Define Agent: Run the entire pipeline on any available agent.
    agent any 
    
    // 2. Define Tools: Jenkins automatically sets JAVA_HOME and M2_HOME based on these global names.
    tools {
        maven 'myMaven' 
        jdk 'JDK_21' 
    }
    
    // 3. Define Stages
    stages {
        stage('Checkout') {
            steps {
                echo 'Starting SCM Checkout...'
                // Clones the repository
                git url: 'https://github.com/AwwwRyan/maven-demo.git', branch: 'main'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Running Maven Build (Clean & Package)...'
                // Executes a clean build. 'mvn' is available because of the 'tools' directive.
                sh 'mvn -B clean package -DskipTests' 
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running Unit Tests...'
                // Executes tests
                sh 'mvn test' 
                // Publishes the JUnit reports for visualization in the Jenkins UI
                junit '**/target/surefire-reports/*.xml' 
            }
        }
        
        stage('Deploy') {
            // This stage runs only if previous stages were successful
            steps {
                echo 'Deployment Simulated: Archiving JAR/WAR artifact...'
                // Archives the generated artifact 
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                // Placeholder for actual deployment command
                sh 'echo "Simulating deployment of artifact to target server..."'
            }
        }
    }
    
    // 4. Post-Actions
    post {
        always {
            // Always cleans up the workspace on the agent
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded! Artifact is ready for delivery.'
        }
        failure {
            echo 'Pipeline failed. Check build and test reports.'
        }
    }
}