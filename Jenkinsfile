pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: buildah
            image: quay.io/buildah/stable:v1.35.4
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
           sh 'buildah build -t harbor.kube.local/public/automation:v0'
      }
    }
  }
}
