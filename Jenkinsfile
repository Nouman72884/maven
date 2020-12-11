pipeline {
    agent any
    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/Nouman72884/maven.git"
            }
        }
        stage ('Artifactory Configuration') {
            steps {
                rtMavenDeployer (
                    id: "deployer",
                    serverId: "jenkins-artifactory",
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local",
                    properties: ['key1=value1', 'key2=value2']
                )

                rtMavenResolver (
                    id: "resolver",
                    serverId: "jenkins-artifactory",
                    releaseRepo: "libs-release",
                    snapshotRepo: "libs-snapshots"
                )
            }
        }
        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'maven', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "deployer",
                    resolverId: "resolver"
                )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "jenkins-artifactory"
                )

                rtAddInteractivePromotion (
                    serverId: "jenkins-artifactory",
                    buildName: JOB_NAME,
                    buildNumber: BUILD_NUMBER
                )
            }
        }
    }
}