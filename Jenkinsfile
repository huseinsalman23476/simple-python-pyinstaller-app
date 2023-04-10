node {
	withDockerContainer('python:2-alpine') {
		stage('Build') {
			sh 'python -m py_compile sources/add2vals.py sources/calc.py'
		}
	}
	withDockerContainer('qnib/pytest') {
		stage('Test') {
			try {
				sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
			}
			finally {
				junit 'test-reports/results.xml'
			}		
		}
	}
	withDockerContainer('cdrx/pyinstaller-linux:python2') {
		stage('Deliver') {
			try {
				sh 'pyinstaller --onefile sources/add2vals.py'
			}
			finally {
				if (currentBuild.result == 'SUCCESS') {
					archiveArtifacts 'dist/add2vals'
				} 
			}		
		}
	}
}