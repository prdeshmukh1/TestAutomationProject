pipeline {
	    agent any
	

	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '0'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://cloud.uipath.com/accelirateuipcl/AccelirateOrchCloud/orchestrator_/"
	        UIPATH_ORCH_LOGICAL_NAME = "accelirateuipcl"
	        UIPATH_ORCH_TENANT_NAME = "AccelirateOrchCloud"
	        UIPATH_ORCH_FOLDER_NAME = "Test Case Prashant"
	    }
	

	    stages {
	

	        // Printing Basic Information
	        stage('Preparing'){
	            steps {
	                echo "Jenkins Home ${env.JENKINS_HOME}"
	                echo "Jenkins URL ${env.JENKINS_URL}"
	                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
	                echo "Jenkins JOB Name ${env.JOB_NAME}"
	                echo "GitHub BranhName ${env.BRANCH_NAME}"
	                checkout scm
	

	            }
	        }
	

	         // Build Stages
	        stage('Build') {
	            steps {
	                echo "Building..with ${WORKSPACE}"
	                UiPathPack (
	                      outputPath: "Output\\${env.BUILD_NUMBER}",
	                      projectJsonPath: "project.json",
	                      version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
	                      useOrchestrator: false,
						  traceLevel: 'None'
	        )
	            }
	        }
			
			
	         // Test Stages
	        stage('Test') {
	            steps {
	                echo 'Testing..the workflow...'

						UiPathTest (
									attachRobotLogs: true, 
							//		credentials: UserPass('8a5a1d60-1306-4044-b8f1-51f6ce4a1685'), 
									credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: "8a5a1d60-1306-4044-b8f1-51f6ce4a1685"],
									folderName: "${UIPATH_ORCH_FOLDER_NAME}", 
									orchestratorAddress: "${UIPATH_ORCH_URL}", 
									orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}", 
									parametersFilePath: '', projectUrl: '',
									repositoryBranch: 'main', 
									repositoryCommit: '', 
									repositoryType: 'git', 
									repositoryUrl: "https://github.com/prdeshmukh1/TestAutomationProject", 
									testResultsOutputPath: 'Build ${BUILD_NUMBER}\\Results.xml', 
									testTarget: TestSet('TS1'), 
									traceLevel: 'Information',
									timeout: 3600
									)
			

       // UiPathTest (
         // testTarget: [$class: 'TestSetEntry', testSet: "TS1"],
         // orchestratorAddress: "https://cloud.uipath.com/accelirateuipcl/AccelirateOrchCloud/orchestrator_",
       //   orchestratorTenant: "AccelirateOrchCloud",
         // folderName: "Test Case Prashant",     
         // testResultsOutputPath: "result.xml",
         // credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: "3f1c2c9e-fa9c-445a-a24f-56d00f88cfce"]
       // )



	            }
	        }
	

	         // Deploy Stages
	        stage('Deploy to UAT') {
	            steps {
	                echo "Deploying ${BRANCH_NAME} to UAT "
	               // UiPathDeploy (
	               // packagePath: "Output\\${env.BUILD_NUMBER}",
	                //orchestratorAddress: "${UIPATH_ORCH_URL}",
	                //orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
	                //folderName: "${UIPATH_ORCH_FOLDER_NAME}",
	                //environments: 'DEV',
	                //credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey']
	                //credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'), 
			//		traceLevel: 'None',
			//		entryPointPaths: 'Main.xaml'
	

	       // )
	            }
	        }
	

	

	         // Deploy to Production Step
	        stage('Deploy to Production') {
	            steps {
	                echo 'Deploy to Production'
	                }
	            }
	    }
	

	    // Options
	    options {
	        // Timeout for pipeline
	        timeout(time:80, unit:'MINUTES')
	        skipDefaultCheckout()
	    }
	

	

	    // 
	    post {
	        success {
	            echo 'Deployment has been completed!'
	        }
	        failure {
	          echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
	        }
	        always {
	            /* Clean workspace if success */
	            cleanWs()
	        }
	    }
	

	}
