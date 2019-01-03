#!groovy

// pod utilisé pour la compilation du projet
podTemplate(label: 'chart-run-pod', containers: [

        // le slave jenkins
        containerTemplate(name: 'jnlp', image: 'jenkinsci/jnlp-slave:alpine'),

        containerTemplate(name: 'helm', image: 'elkouhen/k8s-helm:2.9.1c', ttyEnabled: true, command: 'cat')],

        // montage nécessaire pour que le conteneur docker fonction (Docker In Docker)
        volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]
        ) {

        node('chart-run-pod') {

            properties([
                    parameters([
                            string(defaultValue: '', description: 'Chart à releaser', name: 'chart'),
                            string(defaultValue: '', description: 'Version du chart à deployer', name: 'version')
                    ])
            ])

            stage('CHECKOUT') {
                checkout scm;
            }

            stage('DEPLOY') {

                String command = "./release.sh -c ${params.chart} -v ${params.version}"

                sh "${command}"
            }
        }
}
