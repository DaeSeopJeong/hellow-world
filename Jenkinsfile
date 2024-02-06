node {
    stage('clone') {
        git branch: ‘[브랜치명]’, credentialsId: ‘[credentials 아이디]’, url: ‘[github repository SSH url]’
    }
    stage('Change application.yml') {
        sh '''
            rm ./src/main/resources/application.properties
            cp /home/env/application.properties /var/lib/jenkins/workspace/pipeline/src/main/resources
        '''
    }
    stage('Gradle Build') {
        sh '''
            ./gradlew clean bootJar
        '''
    }
    stage('Send JAR File To Deploy Server'){
        sh'''
            scp -P [포트번호] -i /var/lib/jenkins/.ssh/id_rsa.pem ./build/libs/mongo-log-0.0.1-SNAPSHOT.jar [계정명]@[ip주소]:/var/www/mongo-log/mongo-log-0.0.1-SNAPSHOT.jar
        '''
    }
    stage('Deploy Using systemd'){
        sh'''
            ssh -i /var/lib/jenkins/.ssh/id_rsa.pem [계정명]@[ip주소] "sudo systemctl restart mongo-log.service"
        '''
    }
}
