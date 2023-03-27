podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: gradle
        image: gradle:jdk8
        command:
        - sleep
        args:
        - 99d  
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
      git branch: 'main', url: 'https://github.com/sask787/week8Ex1.git'
      container('centos') {
        stage('Start Calculator') {
		sh '''
                echo 'Initiating Calculator Function'
				curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                chmod +x ./kubectl
                ./kubectl apply -f calculator.yaml
                ./kubectl apply -f hazelcast.yaml
         '''
       
        }
        }
        container('gradle'){

           stage("Execute Acceptance Test and Generate Report") {
                    try {
                        sh '''
        	           chmod +x gradlew
                     ./gradlew acceptanceTest -Dcalculator.url=http://calculator-service:8080
                        '''
                    } catch (Exception E) {
                        echo 'Failure detected in Test Execution'
                    }

                    publishHTML (target: [
                        reportDir: 'build/reports/tests/acceptanceTest',
                        reportFiles: 'index.html',
                        reportName: "Cucumber Execution Report"
                    ])                       
                }
	/*	
		stage("Test using Curl Command") {
		sh '''
                echo 'Test using Curl'
				test $(curl calculator-service:8080/sum?a=6\\&b=2) -eq 8 && echo 'pass' || 'fail'
				test $(curl calculator-service:8080/div?a=6\\&b=6) -eq 1 && echo 'pass' || 'fail'
				test $(curl calculator-service:8080/div?a=6\\&b=0) -eq Infinity && echo 'pass' || 'fail'
         '''
            }
            */
		
      }
    }        

  }
} 