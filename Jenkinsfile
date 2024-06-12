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
        HARBOR_CREDS = credentials('robot')
      }
      steps {
        container('buildah') {
         sh 'echo $HARBOR_CREDS | buildah login -u $HARBOR_CREDS --password-stdin core.harbor.kube.local'
        }
      }
    }
  }
}
