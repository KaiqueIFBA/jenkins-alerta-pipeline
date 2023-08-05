pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Executando etapa de build"
                // Aqui você pode adicionar comandos de build ou testes, se necessário
                    def speedTestResult = sh(script: "speedtest-cli --json", returnStdout: true).trim()

                    def speedTestJson = new groovy.json.JsonSlurperClassic().parseText(speedTestResult)

                    def downloadSpeedMbps = speedTestJson.download / 1000000 // Convert to Mbps

                    echo "Download Speed: ${downloadSpeedMbps} Mbps"
            }
        }
    }
    post {
        always {
            script {
                def emailSubject = "Notificação de Pipeline"
                def emailBody = "O pipeline foi executado com sucesso."
                
                if (currentBuild.resultIsBetterOrEqualTo("FAILURE")) {
                    emailSubject = "Falha no Pipeline"
                    emailBody = "O pipeline falhou durante a execução."
                }

                emailext (
                    subject: emailSubject,
                    body: emailBody,
                    recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                    attachLog: true,
                    compressLog: true,
                    replyTo: "bekahmarques17@gmail.com"
                )
            }
        }
    }
}
