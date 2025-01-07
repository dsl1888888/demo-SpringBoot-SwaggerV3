node(label: 'linux-builder-17'){

    scmVars = checkout(scm)
    gitUrl = scmVars.GIT_URL
    gitBranch = scmVars.GIT_BRANCH

    // read pom file
    pom = readMavenPom(file: 'pom.xml')

    // app information
    appVersion = pom.version.toLowerCase() + '-' + scmVars.GIT_COMMIT[0..4] + '_' + scmVars.GIT_COMMIT[-5..-1]

    if (env.GIT_BRANCH == null) {
        env.GIT_BRANCH = env.BRANCH_NAME
    }

    stage ('unit test') {
        // TODO:
        //     // Use the Maven configured in Jenkins (via Global Tool)
        //     // TODO: Fix unit tests
        //     withMaven(maven: 'maven', options: [artifactsPublisher(disabled: true)]) {
        //         sh "mvn clean verify -Pclover.all --update-snapshots -DskipTests"
        //     }
        //     // TODO: Add threshold for code coverage
        //     step([
        //         $class: 'CloverPublisher',
        //         cloverReportDir: 'target/site',
        //         cloverReportFileName: 'clover.xml'
        //       ])
    }

    stage ('build') {
        // Use the Maven configured in Jenkins (via Global Tool)
        withMaven(maven: 'maven', options: [artifactsPublisher(disabled: true)]) {
            sh "mvn clean deploy --update-snapshots -DskipTests"
        }
    }

}