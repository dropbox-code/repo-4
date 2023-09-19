node("docker-build"){
    stage('Checkout code') {
        checkout scm
    }

    stage('Push helm chart to chartmuseum'){
        withDockerContainer(image: 'registry.internal.logz.io:5000/re-helm-pusher:1.0.3') {
            String[] chartsNames = sh(returnStdout: true, script: "ls charts/").split()
            chartsNames.each{
                print "Push Chart ${it.trim()} "
                try {
                    sh "helm cm-push charts/${it.trim()} logzio-chartmuseum"
                } catch (err) {
                    print "Failed to push chart ${it.trim()}"
                    print err
                }
            }
        }
    }
}