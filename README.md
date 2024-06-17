# koda-docker-versioned-release
Composite action for publishing a versioned release of a koda service's docker image



Example usage in a workflow:
```yml
jobs:
  release-sidekick-ui:
    if: startsWith(github.ref, 'refs/tags/sidekick-ui-')
    runs-on: ubuntu-latest
    steps:
      - uses: enfuse/koda-docker-versioned-release@v1
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          APP_DIRECTORY: ./sidekick
          DOCKERFILE_LOCATION: ./sidekick/Dockerfile
          IMAGE_NAME: koda-sidekick-ui
          GIT_TAG_PREFIX: sidekick-ui
          DOCKER_BUILD_ARGUMENTS: |
            "EXAMPLE_KEY_1=VALUE"
            "EXAMPLE_KEY_2=VALUE"
```
