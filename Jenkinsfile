pipeline {

agent any
tools{
nodejs '14.17.6'
}
stages {
stage("Build") {
steps {
sh " mvn -f  Spring/pom.xml compile"
}
}
stage("Unit tests") {
steps {
sh " mvn -f  Spring/pom.xml test"

}}
stage("Nexus") {
steps {
sh " mvn -f Spring/pom.xml clean package deploy:deploy-file -DgroupId=com.esprit.examen -DartifactId=tpAchatProject -Dversion=0.0.1-SNAPSHOT -DgeneratePom=true -Dpackaging=jar -DrepositoryId=deploymentRepo -Durl=http://192.168.0.10:8081/repository/maven-snapshots/ -Dfile=target/tpAchatProject-0.0.1-SNAPSHOT.jar"

}}
stage("Sonar") {
steps {
sh " mvn  -f Spring/pom.xml clean install sonar:sonar -Dsonar.projectKey=test -Dsonar.host.url=http://192.168.0.10:9000 -Dsonar.login=5c0ac4d3a2f3c05d06ffc0b0a340c0209052cabe"


}}
stage('Building image docker-compose') {
          steps {
          sh "npm --prefix /Angular/ install"
          sh "npm --prefix /Angular/ run build --watch=true"
              sh "docker-compose up -d"
              sh "docker-compose stop"
          }
      }
stage('Deploy our image') {
         steps {
              withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
              sh "docker tag uu_app ahmedchokri/uu_app:uu_app"
              sh "docker push ahmedchokri/uu_app:uu_app"
              sh "docker tag uu_frontend ahmedchokri/uu_frontend:uu_frontend"
              sh "docker push ahmedchokri/uu_frontend:uu_frontend"
         }}
     }
stage('pull and run app') {
              steps {
                  sh "docker-compose -f Spring/docker-compose.yml up -d  "
              }
              }
stage('Cleaning up') {
         steps {
            sh "docker rmi -f uu_app_1"
            sh "docker rmi -f uu_frontend_1"
         }
     }
     stage("Email"){
           steps{
               emailext attachLog: true, body: "${env.BUILD_URL} has result ${currentBuild.result}", compressLog: true, subject: "Status of pipeline: ${currentBuild.fullDisplayName}", to: 'ahmed.chokri@esprit.tn'
           }
       }
}
}
