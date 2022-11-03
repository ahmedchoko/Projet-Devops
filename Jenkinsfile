pipeline {
agent any
stages {
stage("Build") {
steps {
sh " mvn -f  /var/lib/jenkins/workspace/uu/Spring/pom.xml compile"
}
}
stage("Unit tests") {
steps {
sh " mvn -f  /var/lib/jenkins/workspace/uu/Spring/pom.xml test"

}}
stage("Nexus") {
steps {
sh " mvn -f /var/lib/jenkins/workspace/uu/Spring/pom.xml clean package deploy:deploy-file -DgroupId=com.esprit.examen -DartifactId=tpAchatProject -Dversion=0.0.1-SNAPSHOT -DgeneratePom=true -Dpackaging=jar -DrepositoryId=deploymentRepo -Durl=http://192.168.100.170:8081/repository/maven-snapshots/ -Dfile=target/tpAchatProject-0.0.1-SNAPSHOT.jar"

}}
stage("Sonar") {
steps {
sh " mvn  -f /var/lib/jenkins/workspace/uu/Spring/pom.xml clean sonar:sonar -Dsonar.projectKey=test -Dsonar.host.url=http://192.168.100.170:9000 -Dsonar.java.binaries=target/classes -Dsonar.login=deadd25fab00777ef14b0a4bbc057b3a8921f4d6"


}}
}
}
