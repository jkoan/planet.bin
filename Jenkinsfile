pipeline {
    agent any

    parameters {
        string(name: 'PLANET_URL', description: 'URL zur Planet-Datei in osm.pbf')
    }

    stages {
        stage('Prepare') {
            steps {
                cleanWs()
            }
        }

        stage('Install Maptool') {
            steps {
                sh """
                curl -L "https://github.com/navit-gps/dependencies/raw/master/gh-actions-mapserver/navit_trunk_1ed7f1b.sh" -o navit_trunk_1ed7f1b.sh
                chmod +x navit_trunk_1ed7f1b.sh
                mkdir navit
                ./navit_trunk_1ed7f1b.sh --skip-license --prefix=navit
                """
            }
        }
        
        stage('Download Planet') {
            steps {
                sh """
                curl -L "${PLANET_URL}" -o planet.osm.pbf
                """
            }
        }

        stage('Generate Binfile') {
            steps {
                sh """
                ./navit/bin/maptool --64bit --protobuf --slice-size 4294967296 -i planet.osm.pbf planet.bin
                """
            }
        }
    }
}
