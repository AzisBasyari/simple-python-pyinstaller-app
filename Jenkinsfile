node {
    stage("Check Repository") { 
        checkout scm
    }
    stage("Build"){
        docker.image("python:2-alpine").inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage("Test"){
        try {
            docker.image("qnib/pytest").inside {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
        } catch (Exception e) {
            error "Pipeline gagal pada stage Test: ${e.message}"
        } finally {
            junit 'test-reports/results.xml'
        }
    }
}