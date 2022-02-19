# Jenkins

# Installation

- Pull the Jenkins image from DockerHub.
- map port

# Implement CI/CD Pipeline in Jenkins

- Install Bitbucket plugin for jenkins
    - Click on manage Jenkins
    - Click on manage plugins
    - Search "Bitbucket" in available plugins
    - Install and restart
- Create new job
- Name the job
- Choose Freestyle project
- From the source management tab
    - Type the repository URL:
    
    <aside>
    💡 For example: `https://bitbucket.org/esid-team/esid_server.git`
    
    </aside>
    
    - Select the essential credentials to access the repository
- From the build trigger tab you can specify when to trigger the job
- From the build tab you can specify what to do

# Jenkins files

We can implement a pipeline without Jenkins GUI and with using `Jenkinsfile`.

There is two kind of Jenkins file pipeline.

- Scripted
    - It was the first syntax.
    - It is more powerful.
    - It is not easy to get started.
    - It hast high flexibility.
- Declarative
    - It is easier to get started.
    - It has some pre-defined structures.

## Declarative pipeline

```groovy
pipeline {
	
	agent any

	stages {
	
		stage("build") {

			steps {

			}
		}
	}
}
```

### agent

 Where to execute the pipeline.

### stages

Contains some stages and steps to do the job. It usually consists of build, test, and deploy stages at the beginning.

```groovy
pipeline {
	
	agent any

	stages {
	
		stage("build") {

			steps {
				echo 'building the application...'
			}
		}

		stage("test") {

			steps {
				echo 'testing the application...'
			}
		}

		stage("deploy") {

			steps {
				echo 'deploying the application...'
			}
		}
	}
}
```

You can use conditions with when.

There are some environment variables that are provided in the Jenkins file. You can also have your own environment variables. see environment section.

<aside>
💡 Note: You can find all of the variables in path `jenkins-host/env-vars.html`

</aside>

```groovy
CODE_CHANGES = getGitChanges()
pipeline {
	
	agent any

	stages {
	
		stage("build") {

			steps {
				when {
					expression{
						(BRANCH_NAME == 'dev' || BRANCH_NAME='master') && CODE_CHANGES == true
					}
				}
				echo 'building the application...'
			}
		}

		stage("test") {

			steps {
				when {
					expression{
						BRANCH_NAME == 'dev' || BRANCH_NAME='master'
					}
				}
				echo 'testing the application...'
			}
		}

		stage("deploy") {

			steps {
				echo 'deploying the application...'
			}
		}
	}
}
```

### environment

You can define your variables here. They are accessible from everywhere in your Jenkins file.

```groovy
pipeline {
	
	agent any

	environment {
		VERSION = '1.2.3'
	}

	stages {
	
		stage("build") {

			steps {
				echo "building the application.v${VERSION}..."
			}
		}

		stage("test") {

			steps {
				echo 'testing the application...'
			}
		}

		stage("deploy") {

			steps {
				echo 'deploying the application...'
			}
		}
	}
}
```

### Credentials

It looks like the environment variables, but they used as credentials and we should define them in Jenkins GUI. It is needed to install the "Credential Binding" Plugin. Here is the usage.

```groovy
pipeline {
	
	agent any

	environment {
		VERSION = '1.2.3'
		A_CREDENTIAL = credentials('credential_id')
	}

	stages {
	
		stage("build") {

			steps {
				echo "building the application.v${VERSION}..."
			}
		}

		stage("test") {

			steps {
				echo 'testing the application...'
			}
		}

		stage("deploy") {

			steps {
				withCredentials([
					usernamePassword(credentials: 'credential_id', username: USER, pass: PWD)
				]){
					// some codes here that use username and pass
				}
				echo 'deploying the application...'
			}
		}
	}
}
```

### tools

Tools like Maven, Gradle, yarn, or... are needed for the next stages. The tools section is for this purpose. 

```groovy
pipeline {
	
	agent any

	tools {
		maven 'Maven'
	}

	stages {
	
		stage("build") {

			steps {
				echo 'building the application...'
			}
		}

		stage("test") {

			steps {
				echo 'testing the application...'
			}
		}

		stage("deploy") {

			steps {
				echo 'deploying the application...'
			}
		}
	}
}
```

### parameters

You can parameterize your pipeline and give some inputs to the Jenkins file. There some types here:

 

- `string(name, defaultValue, description)`
- `choice(name, choices, description)`
- `booleanParam(name, defaultValue, description)`

```groovy
pipeline {
	
	agent any

	tools {
		maven 'Maven'
	}

	parameters {
		booleanParam(name: 'shouldExecTest', defaultValue: true, description: '')
	}

	stages {
	
		stage("build") {

			steps {
				echo 'building the application...'
			}
		}

		stage("test") {
			when {
				expression {
					params.shouldExecTest == true
				}
			}
			steps {
				echo 'testing the application...'
			}
		}

		stage("deploy") {

			steps {
				echo 'deploying the application...'
			}
		}
	}
}
```

### script (External Groovy Scripts)

It is possible to separate the Jenkins files to other groovy script files and import them into our Jenkins file. It makes our Jenkins file more readable and reduces the lines of code in the Jenkins files.

```groovy
def gv
pipeline {
	
	agent any

	tools {
		maven 'Maven'
	}

	parameters {
		booleanParam(name: 'shouldExecTest', defaultValue: true, description: '')
	}

	stages {

		stage("init") {
			steps {
				script {
					gv = load "script.groovy"
				}
			}
		}
	
		stage("build") {

			steps {
				script {
					// define variables
					// define functions
					gv.buildApp()
				}
				echo 'building the application...'
			}
		}

		stage("test") {
			when {
				expression {
					params.shouldExecTest == true
				}
			}
			steps {
				echo 'testing the application...'
			}
		}

		stage("deploy") {

			steps {
				echo 'deploying the application...'
			}
		}
	}
}
```

```groovy
def buildApp() {
	echo 'building the application (using external script)'
}

return this
```

### post

It is possible to have a post block to do some tasks after all of the stages. It might consist of more complex conditions. A simple example is like:

```groovy
pipeline {

	agent any

	stages {

		stage("build") {

			steps {
				echo 'building the application...'
			}
		}

		stage("test") {

			steps {
				echo 'testing the application...'
			}
		}

		stage("deploy") {

			steps {
				echo 'deploying the application...'
			}
		}
	}

	post {

		always {
			echo 'this is running after stages...'
		}

	  success {
	   echo 'this is running after success...'
	  }

	  failure {
	   echo 'this is running after failure...'
	  }
	}
}
```

# Scripts

```bash
#!/bin/bash

# Install open jdk
sudo add-apt-repository ppa:openjdk-r/ppa  
sudo apt-get update   
sudo apt-get install openjdk-8-jdk-headless

# Install jenkins
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
```

```bash
#!/bin/bash
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo apt-get install docker-compose
sudo apt-get install pass gnupg2
```

```bash
#!/bin/bash
sudo systemctl daemon-reload
sudo systemctl start jenkins
sudo systemctl status jenkins
```

# Resources

[Complete Jenkins Pipeline Tutorial | Jenkinsfile explained](https://www.youtube.com/watch?v=7KCS70sCoK0)