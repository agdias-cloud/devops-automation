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
    stage('Build with Buildah') {
      steps {
        container('buildah') {
          sh 'buildah build -t harbor.kube.local/library/automation:v0 .'
        }
      }
    }
    stage('buildah login') {
      
      steps {
        container('buildah') {
         sh 'echo $harbor-creds | buildah login -u $harbor-creds --password-stdin harbor.kube.local'
        }
      }
    }
  }
}
