pipeline {
   agent any
	stages {
		stage('Git Checkout'){
			steps {
				git 'https://github.com/madhumita-kundo/Feed-Normaliser.git'
				}
		}
		stage('dependency intallation'){
			steps {
				sh 'pip install -r requirements.txt'
				}
		}
		stage('build'){
			steps {
        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
		}
    		stage('test'){
			steps {
				sh 'python test.py'
				}
		}
		stage('package'){
			steps {
				stash(name: 'compiled-results', includes: 'sources/*.py*')
				}
		}
		stage('publish artifacts to udeploy'){
			  step([$class: 'UCDeployPublisher',
        siteName: '35.242.161.163',
        component: [
            $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
            componentName: 'Feed Normaliser Service Component',
         delivery: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Push',
                pushVersion: '$componentName_${BUILD_NUMBER}',
                baseDir: '\\workspace\\web\\targets',
                fileIncludePatterns: '*.py',
                fileExcludePatterns: '',
                pushProperties: 'jenkins.server=Local\njenkins.reviewed=false',
                pushDescription: 'Pushed from Jenkins',
                pushIncremental: false
            ]
        ]
    ])
}
		stage('deploy to UcDeploy'){
			 step([$class: 'UCDeployPublisher',
        siteName: '35.242.161.163',
        component: [
            $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
            componentName: 'Feed Normaliser Service Component'
        ],
        deploy: [
            $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeployHelper$DeployBlock',
            deployApp: 'Full Application',
            deployEnv: 'DEV',
            deployProc: 'Deploy Application',
            deployVersions: '${component_name}_${BUILD_NUMBER}',
            deployOnlyChanged: false
        ]
    ])
}
	}
}
