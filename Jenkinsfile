pipeline {
    agent any

    triggers {
        cron('H/5 * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                echo '=== Step 1: Retrieve source code from GitHub ==='
                bat '''
                    if exist "%WORKSPACE%\\depot" rmdir /S /Q "%WORKSPACE%\\depot"
                    git clone -b main https://github.com/badiesaadi/TP-Jenkins-Bonjour.git "%WORKSPACE%\\depot"
                '''
            }
        }

        stage('Compile') {
            steps {
                echo '=== Step 2: Compile Java program ==='
                bat '''
                    if not exist "%WORKSPACE%\\classes" mkdir "%WORKSPACE%\\classes"
                    javac -d "%WORKSPACE%\\classes" "%WORKSPACE%\\depot\\Bonjour.java"
                '''
            }
        }

        stage('Execute') {
            steps {
                echo '=== Step 3: Run compiled program ==='
                bat '''
                    java -cp "%WORKSPACE%\\classes" Bonjour
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo '=== Step 4: Deploy locally ==='
                bat '''
                    if not exist "C:\\TP-Jenkins\\production" mkdir "C:\\TP-Jenkins\\production"
                    copy /Y "%WORKSPACE%\\classes\\Bonjour.class" "C:\\TP-Jenkins\\production\\Bonjour.class"
                    java -cp "C:\\TP-Jenkins\\production" Bonjour
                '''
            }
        }
    }

    post {
        always {
            echo 'Execution completed at: ' + new Date().toString()
        }
    }
}