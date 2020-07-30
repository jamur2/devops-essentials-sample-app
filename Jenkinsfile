// Build a Maven project using the standard image and Scripted syntax.
// Rather than inline YAML, you could use: yaml: readTrusted('jenkins-pod.yaml')
// Or, to avoid YAML: containers: [containerTemplate(name: 'maven', image: 'maven:3.6.3-jdk-8', command: 'sleep', args: 'infinity')]
POD_YAML='''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3.6.3-jdk-8
    command:
    - sleep
    args:
    - infinity
'''

pipeline {
    agent {
        kubernetes {
            yaml POD_YAML
        }
    }
    stages {
        stage ("Test") {
            steps {
                container('maven') {
                    sh 'mvn -B -ntp -Dmaven.test.failure.ignore verify'
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts '**/target/*.jar'
                }
            }
        }
    }
}