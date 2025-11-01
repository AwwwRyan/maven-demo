// Jenkinsfile (Scripted Pipeline)
node {
    // Stage 1: Checkout (Using your confirmed repository URL)
    stage('Checkout') {
        echo 'Checking out code...'
        // The authenticated user's credentials (if needed) are usually handled by the Jenkins job configuration.
        git url: 'https://github.com/AwwwRyan/maven-demo.git', branch: 'main'
    }

    // Allocate the Maven tool using the name configured in Jenkins
    def mvnHome = tool 'myMaven' 
    
    // Set up the environment (MAVEN_HOME and PATH) using the Maven tool.
    // NOTE: Scripted pipelines require manual tool setup via `withEnv`.
    withEnv(["PATH+MAVEN=${mvnHome}/bin", "M2_HOME=${mvnHome}"]) {
        // We rely on the system environment (or a prior `node` call's environment) 
        // to provide the JDK_21, as `tool 'jdkName'` is less idiomatic in scripted syntax 
        // for simple execution than in declarative.

        // Stage 2: Build
        stage('Build') {
            echo 'Building application...'
            // The shell step runs the Maven command, which uses the environment variables set above.
            sh 'mvn -B clean package -DskipTests'
        }

        // Stage 3: Test
        stage('Test') {
            echo 'Running tests...'
            try {
                sh 'mvn test'
            } catch (e) {
                // Allows the pipeline to proceed to post-actions even if tests fail (marks as UNSTABLE)
                currentBuild.result = 'UNSTABLE'
                echo "Tests failed, marking build as UNSTABLE."
            }
            // Publish JUnit test reports for visualization in Jenkins UI
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
