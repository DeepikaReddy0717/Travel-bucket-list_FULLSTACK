pipeline {
    agent any

    stages {

        // ===== FRONTEND BUILD =====
        stage('Build Frontend') {
            steps {
                echo "🔹 Building Frontend..."
                dir('FrontEnd/travel-bucketlist') {
                    bat 'npm install'
                    bat 'npm run build'
                }
            }
        }

        // ===== FRONTEND DEPLOY =====
        stage('Deploy Frontend to Tomcat') {
            steps {
                echo "🔹 Deploying Frontend to Tomcat..."
                bat '''
                REM Remove old frontend folder if exists
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\travel-bucket-list-frontend" (
                    rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\travel-bucket-list-frontend"
                )

                REM Create new folder
                mkdir "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\travel-bucket-list-frontend"

                REM Copy build files (React build output is usually 'build')
                xcopy /E /I /Y FrontEnd\\travel-bucketlist\\dist\\* "C:\Program Files\Apache Software Foundation\Tomcat 10.1\webapps\travel-bucket-list-frontend"

                '''
            }
        }

        // ===== BACKEND BUILD =====
        stage('Build Backend') {
            steps {
                echo "🔹 Building Backend..."
                dir('BackEnd/travel-bucketlist-backend') {
                    bat 'mvn clean package -DskipTests'
                }
            }
        }

        // ===== BACKEND DEPLOY =====
        stage('Deploy Backend to Tomcat') {
            steps {
                echo "🔹 Deploying Backend to Tomcat..."
                bat '''
                REM Remove old backend deployment if exists
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\travel-bucket-list-backend.war" (
                    del /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\travel-bucket-list-backend.war"
                )
                if exist "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\travel-bucket-list-backend" (
                    rmdir /S /Q "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\travel-bucket-list-backend"
                )

                REM Copy new WAR file (match your pom.xml finalName if needed)
                copy "BackEnd\\travel-bucketlist-backend\\target\\travel-bucketlist-backend-0.0.1-SNAPSHOT.war" "C:\\Program Files\\Apache Software Foundation\\Tomcat 10.1\\webapps\\travel-bucket-list-backend.war"
                '''
            }
        }

    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Pipeline Failed. Check Jenkins logs.'
        }
    }
}
