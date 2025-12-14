pipeline {
    agent any

    parameters {
        string(name: 'PLANET_URL', description: 'URL zur Planet-Datei')
    }

    environment {
        BUILD_DIR = "build/${env.BUILD_NUMBER}"
    }

    stages {
        stage('Prepare') {
            steps {
                sh """
                mkdir -p "${BUILD_DIR}"
                """
            }
        }

        stage('Download Planet') {
            steps {
                sh """
                cd "${BUILD_DIR}"
                curl -L "${PLANET_URL}" -o planet.osm.pbf
                """
            }
        }

        stage('Generate Binfile') {
            steps {
                sh """
                cd "${BUILD_DIR}"
                ./maptool planet.osm.pbf
                """
            }
        }
    }
}
