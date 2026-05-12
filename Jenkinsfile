pipeline {
    agent any
    stages {
        stage('Clean') {
            steps {
                sh 'mvn clean'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test org.jacoco:jacoco-maven-plugin:0.8.9:report -Dmaven.test.failure.ignore=true'
            }
        }
        stage('Install') {
            steps {
                sh 'mvn install -DskipTests'
            }
        }
        stage('PMD') {
            steps {
                sh 'mvn pmd:pmd'
            }
        }
        stage('Javadoc') {
            steps {
                sh 'mvn javadoc:javadoc -Dmaven.javadoc.failOnError=false'
            }
        }
        // Site 阶段暂时跳过，maven-site-plugin 版本不兼容
        // stage('Site') {
        //     steps {
        //         sh 'mvn site'
        //     }
        // }
        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
    }
    post {
        always {
            // 如果跳过了 site，这里也要去掉 site 产物，否则 archiveArtifacts 会报错找不到文件
            // archiveArtifacts artifacts: '**/target/site/**/*.*', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.jar', fingerprint: true
            archiveArtifacts artifacts: '**/target/**/*.war', fingerprint: true
            junit '**/target/surefire-reports/*.xml'
        }
    }
}
