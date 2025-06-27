# koda-docker-versioned-release
Composite action for publishing a versioned release of a koda service's docker image

In order to publish a versioned release:
  1) Stage, commit, and push your Dockerfile
  2) Run `git tag` in order to see a list of tags
  3) Run `git tag [INSERT NEW TAG HERE]`, ensuring you follow the same format as the tags in the list for your new tag
      * (for example) `git tag koda-depth-api-v1.0.2`
  5) Run `git push origin [INSERT NEW TAG HERE]`



Example usage in a workflow:
```yml
name: Release Container Image for the Sidekick UI

on:
  push:
    tags:
      - 'sidekick-ui-[0-9].[0-9].[0-9]*'

jobs:
  release-sidekick-ui:
    if: startsWith(github.ref, 'refs/tags/sidekick-ui-')
    runs-on: [self-hosted,linux,koda]
    steps:
      - uses: enfuse/koda-docker-versioned-release@main
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          APP_DIRECTORY: ./sidekick
          DOCKERFILE_LOCATION: ./sidekick/Dockerfile
          IMAGE_NAME: koda-sidekick-ui
          GIT_TAG_PREFIX: sidekick-ui
          DOCKER_BUILD_ARGUMENTS: |
            "HF_TOKEN=${{ secrets.HF_TOKEN }}"
            "NVIDIA_API_KEY=${{ secrets.NVIDIA_API_KEY }}"
          INTEGRATION_TEST_SCRIPT_PATH: ./test/run_integration_tests.sh
          DOCKER_BUILD_NO_CACHE: false
```

# Cache Docker layers on self hosted runner:
https://docs.docker.com/build/ci/github-actions/cache/#local-cache
