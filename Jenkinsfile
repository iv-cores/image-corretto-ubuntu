import org.jenkinsci.plugins.pipeline.modeldefinition.Utils

// Define parameters in the job configuration
properties([
    parameters([
        booleanParam(name: 'publish docker', defaultValue: env.BRANCH_IS_PRIMARY, description: 'publish to the docker repository')
        string(name: 'publish tags', defaultValue: 'latest, 21-jammy', description: 'comma seperated list of tags to publish')
    ])
])


node {
    def isPublishToDocker = params['publish docker'] ?: false
    def tags = splitTags(params['tags'])
    def imageName = "corretto-ubuntu"

    def image = null
    stage ("build docker") {
        image = docker.build("${imageName}:latest", "--file ./Dockerfile .")
    }

    stage ("publish docker") {
        if(!isPublishToDocker) {
            Utils.markStageSkippedForConditional(STAGE_NAME)
            return
        }

        docker.withRegistry(registryUrl, 'docker-hub-credentials') {
            tags.each { tag ->
                try {
                    image.push(tag)
                    println "Tag ${tag} pushed successfully."
                } catch (Exception e) {
                    println "Failed to push tag ${tag}: ${e.message}"
                }
            }
        }
    }
}

static List<String> splitTags(String input) {
    if (input == null || input.trim().isEmpty()) {
        return ["latest"]
    }
    return input.split(',').collect { it.trim() }
}
