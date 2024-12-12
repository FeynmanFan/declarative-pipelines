pipeline{
    agent any

    environment{
        ARTIFACT_SOURCE_DIRECTORY = "tests/*.xml"
        ORG = 'OurWebCompany'
        PROJECT = 'OurNewApplication'
        PIPELINE = '50'
        PAT = credentials('ado-pat')
    }
    stages{
        stage('Build'){
            steps{
                echo 'Build the code'
            }
        }
        stage('Test'){
            steps{
                echo 'Execute unit tests'
                sh 'mkdir -p tests && echo "test results" > tests/testresults.xml'
                // error "Broken tests break the build"
            }
        }
    }
    post{
        always{
            archiveArtifacts artifacts: ARTIFACT_SOURCE_DIRECTORY, followSymlinks: false

            echo 'Upload the artifact to Azure Storage'

            script{
                //POST https://dev.azure.com/{organization}/{project}/_apis/build/builds?api-version=6.1-preview.7

                def targetURL = "https://dev.azure.com/OurWebCompany/OurNewApplication/_apis/build/builds?definitionId=50&api-version=7.1"

                def authHeader = 'Authorization: Basic $(echo -n :$PAT | base64)' 

                sh "curl -X POST --header 'Content-Type: application/json' --header '${authHeader}' --url 'https://dev.azure.com/OurWebCompany/OurNewApplication/_apis/build/builds?definitionId=50&api-version=7.1'"
            }
        }
    }
}