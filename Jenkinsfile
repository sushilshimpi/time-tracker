node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
	  echo 'hello from Pipeline -------------------->'
      git 'https://github.com/sushilshimpi/time-tracker.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = '/usr/lib/mvn'
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean package/)
      }
   }
   stage('UnitTest') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
      
       // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean test"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore clean test/)
      }
   }
   stage('StaticQualityCheck') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
      
       // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore site"
      } else {
         bat(/"${mvnHome}\bin\mvn" -Dmaven.test.failure.ignore site/)
      }
	  
	  step([$class: 'hudson.plugins.checkstyle.CheckStylePublisher', pattern: '**/target/checkstyle-result.xml', unstableTotalAll:'0',unhealthy:'30', healthy:'10'])
	  step([$class: 'PmdPublisher', pattern: '**/target/pmd.xml'])
	  step([$class: 'FindBugsPublisher', pattern: '**/findbugsXml.xml'])
	  
	  publishHTML (target: [
      allowMissing: false,
      alwaysLinkToLastBuild: false,
      keepAll: true,
      reportDir: 'target/site',
      reportFiles: 'index.html',
      reportName: "Site Report"
    ])
   }
}