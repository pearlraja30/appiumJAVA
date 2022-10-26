pipeline {
    agent any


    stages {
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/balavu/appiumJAVA.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "artifactory_1",
                    url: 'http://3.140.201.29:8081/artifactory' ,
                    credentialsId: 'artifactory'
                )

                rtGradleDeployer (
                    id: "GRADLE_DEPLOYER",
                    serverId: "artifactory_1",
                    repo: test-gradle-dev-local,
                    excludePatterns: ["*.war"],
                )

                rtGradleResolver (
                    id: "GRADLE_RESOLVER",
                    serverId: "artifactory_1",
                    repo: test-gradle-dev-local
                )
            }
        }

        stage ('Config Build Info') {
            steps {
                rtBuildInfo (
                    captureEnv: true,
                    includeEnvPatterns: ["*"],
                    excludeEnvPatterns: ["DONT_COLLECT*"]
                )
            }
        }

        stage ('Exec Gradle') {
            steps {
                rtGradleRun (
                    usesPlugin: true, // Artifactory plugin already defined in build script
                    useWrapper: true,
                    tool: gradle693, // Tool name from Jenkins configuration
                    rootDir: "gradle-examples/gradle-example/",
                    tasks: 'clean artifactoryPublish',
                    deployerId: "GRADLE_DEPLOYER",
                    resolverId: "GRADLE_RESOLVER"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "artifactory_1"
                )
            }
        }
    }
}
