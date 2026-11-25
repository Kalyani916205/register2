##

Started by user Banoth Bhadramma [Pipeline] Start of Pipeline [Pipeline] node Running on [Jenkins](http://localhost:8888/computer/\(built-in\)/) in C:\ProgramData\Jenkins\\.jenkins\workspace\cicd@2 [Pipeline] { [Pipeline] stage [Pipeline] { (Checkout) [Pipeline] git The recommended git tool is: NONE No credentials specified  > git.exe rev-parse --resolve-git-dir C:\ProgramData\Jenkins\\.jenkins\workspace\cicd@2\\.git # timeout=10 Fetching changes from the remote Git repository  > git.exe config remote.origin.url [https://github.com/Kalyani916205/registration.git](https://github.com/Kalyani916205/registration.git) # timeout=10 Fetching upstream changes from [https://github.com/Kalyani916205/registration.git](https://github.com/Kalyani916205/registration.git)  > git.exe --version # timeout=10  > git --version # 'git version 2.54.0.windows.1'  > git.exe fetch --tags --force --progress -- [https://github.com/Kalyani916205/registration.git](https://github.com/Kalyani916205/registration.git) +refs/heads/\*:refs/remotes/origin/\* # timeout=10  > git.exe rev-parse "refs/remotes/origin/main^{commit}" # timeout=10 Checking out Revision 14c601935b96acbafc52610a9a8223f96a661507 (refs/remotes/origin/main)  > git.exe config core.sparsecheckout # timeout=10  > git.exe checkout -f 14c601935b96acbafc52610a9a8223f96a661507 # timeout=10  > git.exe branch -a -v --no-abbrev # timeout=10  > git.exe branch -D main # timeout=10  > git.exe checkout -b main 14c601935b96acbafc52610a9a8223f96a661507 # timeout=10 Commit message: "update website"  > git.exe rev-list --no-walk 14c601935b96acbafc52610a9a8223f96a661507 # timeout=10 [Pipeline] } [Pipeline] // stage [Pipeline] stage [Pipeline] { (Build) [Pipeline] echo Building the project... [Pipeline] bat  C:\ProgramData\Jenkins\\.jenkins\workspace\cicd@2>dir  Volume in drive C is Windows  Volume Serial Number is 7CE5-D93B   Directory of C:\ProgramData\Jenkins\\.jenkins\workspace\cicd@2  25-11-2026  00:12    \<DIR>          . 25-11-2026  00:12    \<DIR>          .. 25-11-2026  00:12             6,873 registration.html                1 File(s)          6,873 bytes                2 Dir(s)  271,792,996,352 bytes free [Pipeline] } [Pipeline] // stage [Pipeline] stage [Pipeline] { (Test) [Pipeline] echo Testing the project... [Pipeline] bat  C:\ProgramData\Jenkins\\.jenkins\workspace\cicd@2>if exist index.html (echo Test Passed )  else (exit /b 1 )  [Pipeline] } [Pipeline] // stage [Pipeline] stage [Pipeline] { (Delivery) Stage "Delivery" skipped due to earlier failure(s) [Pipeline] getContext [Pipeline] } [Pipeline] // stage [Pipeline] } [Pipeline] // node [Pipeline] End of Pipeline ERROR: script returned exit code 1 Finished: FAILURE

…
Your Jenkins pipeline is working correctly up to the Build stage. The problem is in the Test stage.

Checkout – Success Build – Success

Test – Failed Delivery – Skipped

Why did the error occur?
Your GitHub repository contains:

registration.html

But your Jenkins pipeline is checking for:

index.html

Your current command is:

bat 'if exist index.html (echo Test Passed) else (exit /b 1)'
Since index.html does not exist, the test fails.

Solution: Change the Test stage
Replace your existing Test stage with this:

stage('Test') {
    steps {
        echo 'Testing the project...'
        bat 'if exist registration.html (echo Test Passed) else (exit /b 1)'
    }
}
Also change the Delivery stage:

stage('Delivery') {
    steps {
        archiveArtifacts artifacts: 'registration.html'
        echo 'Project delivered successfully!'
    }
}
Now follow these steps
Open Jenkins.

Open your cicd pipeline.

Click Configure.

Go to Pipeline Script.

Replace the old Test and Delivery stages with the corrected code.

Click Save.

Click Build Now.

Open Console Output.

Expected output
Pipeline Result
Checkout

Success
Build

Success
Test

Success
Delivery

Success
Finished: SUCCESS
Result: Your Jenkins CI/CD pipeline should execute successfully after correcting the filename.

give full code
Here is the complete Jenkins Pipeline Script for your GitHub repository. I used your actual repository name and HTML filename (registration.html).

Complete Jenkinsfile / Pipeline Script
Copy the entire code into Jenkins → Configure → Pipeline → Pipeline Script.


pipeline {
    agent any

    triggers {
        pollSCM('H/2 * * * *')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Kalyani916205/registration.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                bat 'dir'
                echo 'Build completed successfully!'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing the project...'
                bat 'if exist registration.html (echo Test Passed) else (exit /b 1)'
            }
        }

        stage('Delivery') {
            steps {
                archiveArtifacts artifacts: 'registration.html'
                echo 'Project delivered successfully!'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline Executed Successfully!'
        }

        failure {
            echo 'CI/CD Pipeline Failed!'
        }
    }
}
Steps to execute
Open Jenkins (http://localhost:8888).

Select your cicd project.

Click Configure.

Select Pipeline.

Delete the old script.

Paste the above complete code.

Click Save.

Click Build Now.

Open Console Output.

Expected result
Jenkins CI/CD
Checkout
SUCCESS
Build
SUCCESS
Test
SUCCESS
Delivery
SUCCESS
Finished: SUCCESS
Note: pollSCM('H/2 * * * *') checks GitHub approximately every two minutes for new changes. This pipeline demonstrates automatic checkout, build, testing, and artifact delivery.

Started by user Banoth Bhadramma [Pipeline] Start of Pipeline [Pipeline] node Running on [Jenkins](http://localhost:8888/computer/\(built-in\)/) in C:\ProgramData\Jenkins\\.jenkins\workspace\cicd [Pipeline] { [Pipeline] stage [Pipeline] { (Checkout) [Pipeline] git The recommended git tool is: NONE No credentials specified  > git.exe rev-parse --resolve-git-dir C:\ProgramData\Jenkins\\.jenkins\workspace\cicd\\.git # timeout=10 Fetching changes from the remote Git repository  > git.exe config remote.origin.url [https://github.com/Kalyani916205/registration.git](https://github.com/Kalyani916205/registration.git) # timeout=10 Fetching upstream changes from [https://github.com/Kalyani916205/registration.git](https://github.com/Kalyani916205/registration.git)  > git.exe --version # timeout=10  > git --version # 'git version 2.54.0.windows.1'  > git.exe fetch --tags --force --progress -- [https://github.com/Kalyani916205/registration.git](https://github.com/Kalyani916205/registration.git) +refs/heads/\*:refs/remotes/origin/\* # timeout=10  > git.exe rev-parse "refs/remotes/origin/main^{commit}" # timeout=10 Checking out Revision 14c601935b96acbafc52610a9a8223f96a661507 (refs/remotes/origin/main)  > git.exe config core.sparsecheckout # timeout=10  > git.exe checkout -f 14c601935b96acbafc52610a9a8223f96a661507 # timeout=10  > git.exe branch -a -v --no-abbrev # timeout=10  > git.exe branch -D main # timeout=10  > git.exe checkout -b main 14c601935b96acbafc52610a9a8223f96a661507 # timeout=10 Commit message: "update website"  > git.exe rev-list --no-walk 14c601935b96acbafc52610a9a8223f96a661507 # timeout=10 [Pipeline] } [Pipeline] // stage [Pipeline] stage [Pipeline] { (Build) [Pipeline] echo Building the project... [Pipeline] bat  C:\ProgramData\Jenkins\\.jenkins\workspace\cicd>dir  Volume in drive C is Windows  Volume Serial Number is 7CE5-D93B   Directory of C:\ProgramData\Jenkins\\.jenkins\workspace\cicd  24-11-2026  23:54    \<DIR>          . 25-11-2026  00:12    \<DIR>          .. 24-11-2026  23:54             6,873 registration.html                1 File(s)          6,873 bytes                2 Dir(s)  271,790,182,400 bytes free [Pipeline] echo Build completed successfully! [Pipeline] } [Pipeline] // stage [Pipeline] stage [Pipeline] { (Test) [Pipeline] echo Testing the project... [Pipeline] bat  C:\ProgramData\Jenkins\\.jenkins\workspace\cicd>if exist registration.html (echo Test Passed )  else (exit /b 1 )  Test Passed [Pipeline] } [Pipeline] // stage [Pipeline] stage [Pipeline] { (Delivery) [Pipeline] archiveArtifacts Archiving artifacts [Pipeline] echo Project delivered successfully! [Pipeline] } [Pipeline] // stage [Pipeline] stage [Pipeline] { (Declarative: Post Actions) [Pipeline] echo CI/CD Pipeline Executed Successfully! [Pipeline] } [Pipeline] // stage [Pipeline] } [Pipeline] // node [Pipeline] End of Pipeline Finished: SUCCESS 

…
SUCCESS – Pipeline Executed Successfully
Excellent! Your Jenkins CI/CD pipeline is now working correctly. There are no errors in the latest Console Output.

Stage-wise execution results
Stage

Result

Checkout

SUCCESS

Build

SUCCESS

Test

SUCCESS

Delivery

SUCCESS

Post Actions

SUCCESS

What happened?
Checkout: Jenkins successfully downloaded your project from GitHub.

Build: Jenkins identified registration.html in the workspace.

Test: Jenkins checked whether registration.html exists. Test Passed.

Delivery: Jenkins successfully archived registration.html.

Post Actions: Jenkins displayed the successful execution message.

Final output
Building the project...
Build completed successfully!
Testing the project...
Test Passed
Project delivered successfully!
CI/CD Pipeline Executed Successfully!

Finished: SUCCESS
Result for your lab record
Successfully demonstrated Continuous Integration and Continuous Delivery using Jenkins by integrating a GitHub repository with Jenkins. The pipeline performed checkout, build, testing, and artifact delivery successfully.

Your experiment is completed successfully!

One important note: You have demonstrated Continuous Delivery. To demonstrate Continuous Deployment, you would additionally configure automatic deployment to a running web server.





