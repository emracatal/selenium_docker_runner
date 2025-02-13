pipeline{
    agent any

    parameters {
      choice choices: ['chrome', 'firefox'], description: 'Select the browser', name: 'BROWSER'
    }

    stages{
           stage("Start Grid"){
                steps{
                    bat "docker-compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
                }
           }
           stage("Run Test"){
                 steps{
                     bat "docker-compose -f test-suite.yaml up  "
                 }
           }
    }

    post {
        always {
            bat "docker-compose -f grid.yaml down"
            bat "docker-compose -f test-suite.yaml down"

            //archive files
            archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }


}