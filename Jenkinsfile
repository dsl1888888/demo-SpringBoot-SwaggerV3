node(){

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
    stage ('build') {
        // Use the Maven configured in Jenkins (via Global Tool)
        withMaven(maven: 'maven', options: [artifactsPublisher(disabled: true)]) {
            sh "mvn clean install --update-snapshots -DskipTests"
        }
    }

}