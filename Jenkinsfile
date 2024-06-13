pipeline {
  agent {
    kubernetes {
      yaml '''
apiVersion: v1
kind: Pod
metadata:
  name: buildah
spec:
  containers:
  - name: buildah
    image: quay.io/buildah/stable:v1.23.1
    command:
    - cat
    tty: true
    securityContext:
      privileged: true
    volumeMounts:
      - name: varlibcontainers
        mountPath: /var/lib/containers
  volumes:
    - name: varlibcontainers
'''   
    }
  }
  
  options {
    buildDiscarder(logRotator(numToKeepStr: '3'))
    durabilityHint('PERFORMANCE_OPTIMIZED')
    disableConcurrentBuilds()
  }
  
  stages {
    stage('buildah login') {
      environment {
        DH_CREDS = credentials('dockerhub')
      }
      steps {
        container('buildah') {
             sh 'echo $DH_CREDS_PSW | buildah login -u $DH_CREDS_USR --password-stdin docker.io'
        }
      }
    }
  }
}
