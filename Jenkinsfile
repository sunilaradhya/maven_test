pipeline
{
    agent { label 'master' }
    stages
    {
        stage('continuous download')
        {
            steps
            {
                script
                {
                  try
                  {
                     git 'https://github.com/keshavr21/maven_test.git'
                  }
                  catch(Exception e1)
                  {
                     mail bcc: '', body: 'DOWNLOADING CODE IS FAILED', cc: '', from: '', replyTo: '', subject: '', to: 'github@xyz.com'
                     exit(1)
                  }
                }
            }
        }
        stage('continuos integration')
        {
            steps
            {
               script
               {
                 try
                 {
                     sh 'mvn clean package'
                 }
                 catch(Exception e2)
                 {
                     mail bcc: '', body: 'BUILDING CODE IS FAILED', cc: '', from: '', replyTo: '', subject: '', to: 'dev@xyz.com'
                     exit(1)
                 }
               }
                
            }
        }
        stage('continuous deployment')
        {
            steps
            {
               script 
               {
                 try 
                 {
                     deploy adapters: [tomcat9(credentialsId: '212218ef-3936-4650-9659-6d1a636b1c10', path: '', url: 'http://172.31.36.158:8080/')], contextPath: 'testapp', war: '**/*.war'
                 }
                 catch(Exception e3)
                 {
                     mail bcc: '', body: 'BUILDING CODE IS FAILED', cc: '', from: '', replyTo: '', subject: '', to: 'devops@xyz.com'
                     exit(1)
                 }
               }
                
            }
        }
        stage('continuous testing')
        {
            steps
            {
               script
               {
                 try 
                 {
                   git 'https://github.com/keshavr21/test_cases.git'
                   sh 'java -jar $WORKSPACE/testing.jar'
                 }
                 catch(Exception e4)
                 {
                   mail bcc: '', body: 'BUILDING CODE IS FAILED', cc: '', from: '', replyTo: '', subject: '', to: 'testing@xyz.com'
                   exit(1)
                 }
               }
            }
        }
        stage('continuous delivery')
        {
            steps
            {
                deploy adapters: [tomcat9(credentialsId: '212218ef-3936-4650-9659-6d1a636b1c10', path: '', url: 'http://172.31.36.158:8080/')], contextPath: 'prod', war: '**/*.war'
            }
        }

    }
    post
    {
        success
        {
            sh 'echo "THIS STEP WILL RUN when job is success"'
        }
        failure
        {
            mail bcc: '', body: 'CI PIPELINE IS FAILED', cc: '', from: '', replyTo: '', subject: '', to: 'team@xyz.com'
        }
        always
        {
            sh 'echo "THIS STEP WILL ALWAYS RUN"'
        }
        aborted
        {
            sh 'echo "THIS STEP WILL RUN WHEN JOB IS ABORTED"'
        }
    }

}
