pipeline {
    agent any
    def app
    stages {
        stage('Clone repository') {
            /* Let's make sure we have the repository cloned to our workspace */
	    steps {
                checkout scm
	    }
        }

        stage('Build image') {
            /* This builds the actual image; synonymous to
             * docker build on the command line */
	    steps {
                app = docker.build("cankush625/webpage")
	    }
        }

        stage('Test image') {
            /* Ideally, we would run a test framework against our image.
             * Just an example */
	    steps {
                app.inside {
                    sh 'echo "Tests passed"'
                }
	    }
        }

        stage('Run image') {
            /*Run an image to start services*/
	    steps {
                docker.image("cankush625/webpage").run('-p 80:80')
	    }
        }

        stage('Push image') {
            /* Finally, we'll push the image with two tags:
             * First, the incremental build number from Jenkins
             * Second, the 'latest' tag.
             * Pushing multiple tags is cheap, as all the layers are reused. */
	    steps {
                docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                }
	    }
        }
    }

    post {
        success {
            echo "All stages are built successfully!"
        }
    }
}
