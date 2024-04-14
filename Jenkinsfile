podTemplate(yaml: """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: jnlp
    image: jenkins/jnlp-slave:latest
    command:
    - cat
    tty: true
"""
) {
    node(POD_LABEL) {
      container('jnlp') {
        sh "hostname"
      }
    }
}
