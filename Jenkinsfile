podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers: 
      - name: centos
        image: centos
        command:
        - sleep
        args:
        - 99d
''')
 {
  node(POD_LABEL) {

  stage('Start a gradle project') {
      git branch: 'main', url: 'https://github.com/sask787/week9.git'
      container('centos') {
        stage('Start Calculator') {
		sh '''
                echo 'Initiating Calculator Function'
				curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                chmod +x ./kubectl
                ./kubectl apply -f calculator.yaml -n staging
                ./kubectl apply -f hazelcast.yaml -n staging
         '''
       
        }
        }
    }        

  }
} 