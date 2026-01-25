pipeline {
agent any

stages {
stage('chckout scm') {
steps {
checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/amitopenwriteup/javadocker.git']])
}
}
stage('Install sonarqube cli') {
steps {
sh 'sudo apt install unzip -y'
sh 'sudo wget -O sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip'
sh 'sudo unzip -o -q sonar-scanner.zip'
sh 'sudo rm -rf /opt/sonar-scanner'
sh 'sudo mv --force sonar-scanner-5.0.1.3006-linux /opt/sonar-scanner'
sh 'sudo sh -c \'echo "#/bin/bash \nexport PATH=\\\"$PATH:/opt/sonar-scanner/bin\\\"" >/etc/profile.d/sonar-scanner.sh\''
sh 'sudo chmod +x /opt/sonar-scanner/bin/sonar-scanner'
sh '. /etc/profile.d/sonar-scanner.sh'
}
}
stage('Analyzing Code Quality') {
steps {
// Step to analyze code quality with SonarQube
sh '/opt/sonar-scanner/bin/sonar-scanner -Dsonar.projectKey=amitopenwriteup_myproject -Dsonar.organization=amitopenwriteup -Dsonar.qualitygate.wait=false -Dsonar.qualitygate.timeout=300 -Dsonar.sources=src/main/java/ -Dsonar.java.binaries=target/classes -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=1a64d0e9b58af6ee49213886de94bfc3b1cf3661'
}
}
}
}
