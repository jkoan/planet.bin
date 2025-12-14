pipeline {
    agent any

    parameters {
        string(name: 'PLANET_URL', description: 'URL zur Planet-Datei in xml')
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

        stage('Install Maptool') {
            steps {
                sh """
                cd "${BUILD_DIR}"
                curl -L "${PLANET_URL}" "https://github.com/navit-gps/dependencies/raw/master/gh-actions-mapserver/navit_trunk_1ed7f1b.sh" -o navit_trunk_1ed7f1b.sh
                chmod +x navit_trunk_1ed7f1b.sh
                mkdir navit
                ./navit_trunk_1ed7f1b.sh --skip-license --prefix=navit
                """
            }
        }
        
        stage('Download Planet') {
            steps {
                sh """
                cd "${BUILD_DIR}"
                curl -L "${PLANET_URL}" -o planet.osm
                """
            }
        }

        stage('Generate Binfile') {
            steps {
                sh """
                cd "${BUILD_DIR}"
                ./navit/maptool planet.osm.pbf
                """
            }
        }
    }
}
