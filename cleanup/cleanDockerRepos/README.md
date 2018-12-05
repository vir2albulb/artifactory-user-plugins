Artifactory Clean Docker Repos User Plugin
===========================================

This plugin is used to clean Docker repositories based on configured cleanup
policies.

Parameters
----------

- `repos`: A list of repositories to clean. This parameter is required.
- `dryRun`: If this parameter is passed, artifacts will not actually be deleted. Default *false*.


Usage
-----

Cleanup policies are specified as labels on the Docker image. Currently, this
plugin supports the following policies:

- `maxDays`: The maximum number of days a Docker image can exist in an
  Artifactory repository. Any images older than this will be deleted.
- `maxCount`: The maximum number of versions of a particular image which should
  exist. For example, if there are 10 versions of a Docker image and `maxCount`
  is set to 6, the oldest 4 versions of the image will be deleted.

To set these labels for an image, add them to the Dockerfile before building:

``` dockerfile
LABEL com.jfrog.artifactory.retention.maxCount="10"
LABEL com.jfrog.artifactory.retention.maxDays="7"
```

When a Docker image is deployed, Artifactory will automatically create
properties reflecting each of its labels. These properties are read by the
plugin in order to decide on the cleanup policy for the image.

Cleanup can be triggered via a REST endpoint. For example:

``` shell
curl -XPOST -uadmin:password http://localhost:8081/artifactory/api/plugins/execute/cleanDockerRepos?params=repos=example-repo-local
```

A dry run can also be triggered:

``` shell
curl -XPOST -uadmin:password "http://localhost:8081/artifactory/api/plugins/execute/cleanDockerRepos?params=dryRun=true;repos=example-repo-local"
```
