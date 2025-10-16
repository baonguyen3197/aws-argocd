pipeline {
    agent any
    
    // agent {
    //     kubernetes {
    //         yamlFile 'kaniko-builder.yaml'
    //     }
    // }

    environment {
        ARGOCD_SERVER   = 'https://argocd-server.argocd.svc.cluster.local'
        ARGOCD_APP_NAME = 'guestbook-ui'
    }

    stages {
        stage('Argo CD Sync') {
            steps {
                withCredentials([string(credentialsId: 'argocd-creds', variable: 'ARGOCD_AUTH_TOKEN')]) {
                    sh '''
                      set -e
                      # Optional: refresh app cache before sync
                      curl -sk -X POST \
                        -H "Authorization: Bearer ${ARGOCD_AUTH_TOKEN}" \
                        "${ARGOCD_SERVER}/api/v1/applications/${ARGOCD_APP_NAME}/refresh?refresh=hard" || true

                      # Trigger sync
                      curl -sk -X POST \
                        -H "Content-Type: application/json" \
                        -H "Authorization: Bearer ${ARGOCD_AUTH_TOKEN}" \
                        "${ARGOCD_SERVER}/api/v1/applications/${ARGOCD_APP_NAME}/sync"
                    '''
                }
            }
        }
    }
}