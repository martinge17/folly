podTemplate(containers: [
    containerTemplate(name: 'ubuntu', image: 'ubuntu:24.04', command: 'sleep', args: 'infinity'),
  ]) {

    node(POD_LABEL) {
        stage('Build project') {
            git url: 'https://github.com/martinge17/folly.git', branch: 'main'
            container('ubuntu') {
                stage('Install deps') {
                    sh 'apt update && apt install -y python3'
                    sh './build/fbcode_builder/getdeps.py install-system-deps --recursive' 
                }
                stage('Build folly'){
                    sh 'python3 build/fbcode_builder/getdeps.py --allow-system-packages build --src-dir=. folly  --project-install-prefix folly:/usr/local'
                }
                stage('Run tests'){
                    sh 'python3 build/fbcode_builder/getdeps.py --allow-system-packages test --src-dir=. folly  --project-install-prefix folly:/usr/local'
                }
            }
        }
    }
}