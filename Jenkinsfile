pipeline{
	agent any

	tools{
	   jdk 'java-11'
	   maven 'maven'
	}

	stages{
	    stage('Git checkout'){
	       steps{
	          git branch: 'main' , url:'https://github.com/amithachar/ContinousIntegration.git'
	       }
	    }
	    stage('Compile'){
	       steps{
	          sh 'mvn compile'
	       }
	    }
	      stage('Build'){
	       steps{
	          sh 'mvn clean install'
	       }
	    } 
	     stage('Building-Dockerimage'){
	       steps{
	          sh  'docker build -t amithachar/continous-integration:1 .' 
	       }
	    }
        stage('Containerzation'){
           steps{
	        sh'''
	          docker stop c1 || true
	          docker rm c1 || true
	          docker run -it -d --name c1 -p 9000:8080 amithachar/continous-integration:1  
	        '''
	        }
	    }


	}

}
