//library identifier: "pipeline-library@master",
//retriever: modernSCM(
//  [
//    $class: "GitSCMSource",
//    remote: "https://github.com/redhat-cop/pipeline-library.git"
//  ]
//)

openshift.withCluster() {
  env.NAMESPACE = openshift.project()
  env.POM_FILE = env.BUILD_CONTEXT_DIR ? "${env.BUILD_CONTEXT_DIR}/pom.xml" : "pom.xml"
  env.APP_NAME = "${JOB_NAME}".replaceAll(/-build.*/, '')
  echo "Starting Pipeline for ${APP_NAME}..."
  env.BUILD = "${env.NAMESPACE}"
//  env.DEV = "${APP_NAME}-dev"
//  env.STAGE = "${APP_NAME}-stage"
//  env.PROD = "${APP_NAME}-prod"
//  env.https_proxy = "http://revproxytest.oz501.be:3128/"
//  env.http_proxy = "http://revproxytest.oz501.be:3128/"
//  env.HTTP_PROXY = "http://revproxytest.oz501.be:3128/"
  env.MAVEN_MIRROR_URL = "http://nexus.sdlc.gfdi.be/nexus/content/groups/mavenx"
}

pipeline {
  // Use Jenkins Maven slave
  // Jenkins will dynamically provision this as OpenShift Pod
  // All the stages and steps of this Pipeline will be executed on this Pod
  // After Pipeline completes the Pod is killed so every run will have clean
  // workspace
  agent {
    label 'maven'
  }

  // Pipeline Stages start here
  // Requeres at least one stage
  stages {

//    stage('update maven mirror') {
//	steps {
//                sh "cat ~/.m2/settings.xml"
//                sh "sed -i 's|<!-- ### configured mirrors ### -->|<mirror><mirrorOf>*</mirrorOf><url>http://nexus.sdlc.gfdi.be/nexus/content/groups/mavenx</url><id>mirror</id></mirror>|' ~/.m2/settings.xml"
//                sh "cat ~/.m2/settings.xml"
//	}
//    }
//    // Checkout source code
//    // This is required as Pipeline code is originally checkedout to
//    // Jenkins Master but this will also pull this same code to this slave
//    stage('Git Checkout') {
//      steps {
//        // Turn off Git's SSL cert check, uncomment if needed
//        // sh 'git config --global http.sslVerify false'
//        git url: "${APPLICATION_SOURCE_REPO}"
//      }
//    }
//
//    // Run Maven build, skipping tests
//    stage('Build'){
//      steps {
//        sh "mvn clean install -DskipTests=true -f ${POM_FILE}"
//      }
//    }
//
//    // Run Maven unit tests
//    stage('Unit Test'){
//      steps {
//        sh "mvn test -f ${POM_FILE}"
//      }
//    }

    // Build Container Image using the artifacts produced in previous stages
    stage('Build Container Image'){
      steps {
        // Copy the resulting artifacts into common directory
//        sh """
//          ls target/*
//          rm -rf oc-build && mkdir -p oc-build/deployments
//          for t in \$(echo "jar;war;ear" | tr ";" "\\n"); do
//            cp -rfv ./target/*.\$t oc-build/deployments/ 2> /dev/null || echo "No \$t files"
//          done
//        """
//
        // Build container image using local Openshift cluster
        // Giving all the artifacts to OpenShift Binary Build
        // This places your artifacts into right location inside your S2I image
        // if the S2I image supports it.

 	echo " app name : ${APP_NAME}"
       	echo " build : ${BUILD}"

	script {
		openshift.withCluster() { // Use "default" cluster or fallback to OpenShift cluster detection
		    echo "Hello from the project running Jenkins: ${openshift.project()}"

		 // Create a Selector capable of selecting all service accounts in mycluster's default project
		    def saSelector = openshift.selector( 'serviceaccount' )

		    // Prints `oc describe serviceaccount` to Jenkins console
		    saSelector.describe()

		  openshift.selector( [ 'bc/s2i-liberty-binary-app'] ).describe()
			//openshift.startBuild("s2i-liberty-binary-app --from-dir=./oc-build/")
		}
	}

   //   sh "oc start-build s2i-liberty-binary-app --from-dir=./oc-build/ --wait --follow"

  //      binaryBuild(projectName: env.BUILD, buildConfigName: "s2i-liberty-binary-app", artifactsDirectoryName: "oc-build")
      }
    }

//    stage('Promote from Build to Dev') {
//      steps {
//       
//       
//        tagImage(sourceImageName: env.APP_NAME, sourceImagePath: env.BUILD, toImagePath: env.DEV)
//      }
//    }
//
//    stage ('Verify Deployment to Dev') {
//      steps {
//        verifyDeployment(projectName: env.DEV, targetApp: env.APP_NAME)
//      }
//    }
//
//    stage('Promote from Dev to Stage') {
//      steps {
//        tagImage(sourceImageName: env.APP_NAME, sourceImagePath: env.DEV, toImagePath: env.STAGE)
//      }
//    }
//
//    stage ('Verify Deployment to Stage') {
//      steps {
//        verifyDeployment(projectName: env.STAGE, targetApp: env.APP_NAME)
//      }
//    }
//
//    stage('Promotion gate') {
//      steps {
//        script {
//          input message: 'Promote application to Production?'
//        }
//      }
//    }
//
//    stage('Promote from Stage to Prod') {
//      steps {
//        tagImage(sourceImageName: env.APP_NAME, sourceImagePath: env.STAGE, toImagePath: env.PROD)
//      }
//    }
//
//    stage ('Verify Deployment to Prod') {
//      steps {
//        verifyDeployment(projectName: env.PROD, targetApp: env.APP_NAME)
//      }
//    }
  }
}

