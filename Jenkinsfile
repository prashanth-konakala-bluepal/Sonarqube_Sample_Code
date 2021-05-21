pipeline 
        {
            agent any
            stages
                {
                    stage ("Git Checkout")
                            {
                                steps 
                                        {
                                            git branch: 'master', url: 'https://github.com/prashanth-konakala-bluepal/Sonarqube_Sample_Code.git'
                                        }
                            }
                    stage ("Build")
                            {
                                steps
                                        {
                                            // sh 'mvn clean install -f MyWebApp/pom.xml'
                                            sh 'mvn clean package -f pom.xml'
                                        }
                            }
                    stage ("Sonar Analysis")
                            {
                                steps
                                        {
                                            script 
                                                    {
                                                        def scannerHome = tool 'sonarqube';
                                                        withSonarQubeEnv("Sonarqube_Pipeline")
                                                        {
                                                         sh 'mvn clean install -f pom.xml'
                                                         sh " mvn -f pom.xml sonar:sonar \
                                                              -Dsonar.projectName=Sonarqube_Jenkinsfile_Pipeline \
                                                              -Dsonar.projectKey=Sonarqube_Jenkinsfile_Pipeline \
                                                              -Dsonar.login=b86e54949ae3cb448a47fbd9aca44dfc24fc214d "
                                                        }
                                                    }
                                        }
                            }
                    stage ("Code Coverage")
                            {
                               steps 
                                    {
                                      jacoco()       
                                    }
                            }
                    stage("Quality Status Check")
                                {
                                 steps
                                                {
                                                        timeout (time: 3, unit: 'MINUTES')
                                                                {
                                                                        script
                                                                                        {
                                                                                                def qg = waitForQualityGate()
                                                                                                if (qg.status != 'OK')
                                                                                                        {
                                                                                                                error "Pipeline aborted due to Quality Gate Failure: ${qg.status}"
                                                                                                        }
                                                                                                else //(qg.status = 'OK')
                                                                                                        {
                                                                                                                print "Pipeline Executed Successfully: ${qg.status}"
                                                                                                        }
                                                                                        }
                                                                }	
                                                }
                                }
                     stage ("Email Notification")
						{
							steps
									{
										mail bcc: '', body: '''Hi Welcome to Jenkins Email Alerts
										Thanks
										Prashanth''', cc:'', from: '', replyTo:'', subject: 'Jenkins Job', to: 'prashanth.konakala@bluepal.com'
									}
						}
                }
        }
