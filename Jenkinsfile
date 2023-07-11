/*
apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins-slave
  namespace: jenkins
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: jenkins-slave-admin
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: jenkins-slave
  namespace: jenkins
*/

pipeline {

    parameters {
        string(description: "Kubernetes Command", name: "CMD", defaultValue: '')
    }

    agent {
        kubernetes {
            yaml """
                apiVersion: "v1"
                kind: "Pod"
                spec:
                  securityContext:
                    runAsUser: 1001
                    runAsGroup: 1001
                    fsGroup: 1001
                  containers:
                  - command:
                    - "cat"
                    image: "bitnami/kubectl:latest"
                    imagePullPolicy: "IfNotPresent"
                    name: "kubectl"
                    resources: {}
                    tty: true
                  serviceAccountName: jenkins-slave
            """
        }
    }

    stages {

        stage ("Kubectl") {

            steps {

                container ("kubectl") {

                    script {

                        // Apply
                        sh "${CMD}"

                    }

                }

            }

        }

    }

}
