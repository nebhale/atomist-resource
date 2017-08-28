# Atomist CI Resource
Notifies [Atomist][a] about CI lifecycle changes.

## Resource Type Configuration

```yaml
resource_types:
- name: atomist
  type: docker-image
  source:
    repository: nebhale/atomist-resource
```

## Source Configuration

```yaml
resources:
- name: atomist
  type: atomist
  source:
    username: <GITHUB_ORGANIZATION>
    reponame: <GITHUB_REPOSITORY>
    branch:   <BRANCH_NAME>
```

| Parameter | Description
| --------- | -----------
| `username` | The GitHub organization that this resource notifies Atomist about
| `reponame` | The GitHub respository that this resource notifies Atomist about
| `branch` | The branch that this resource notifies Atomist about

## Behavior

### `out`: Notifies Atomist
Notifies Atomist, specifying the lifecycle event.

```yaml
- name: my-task
  plan:
  - get: my-repository
    trigger: true
  - task: unit-test
    file: my-respository/ci/unit-test.yml
    on_failure:
      put: atomist
      params:
        repository: my-repository
        lifecycle:  failure
    on_success:
      put: atomist
      params:
        repository: my-repository
        lifecycle:  success
```

| Parameter | Description
| --------- | -----------
| `repository` | The name of the folder that the source code was checked out to
| `lifecycle` | The Atomist CI lifecycle event to send.  This is configurable for separate success/failure configurations.


## License
This project is released under version 2.0 of the [Apache License][l].

[a]: https://www.atomist.com
[l]: https://www.apache.org/licenses/LICENSE-2.0
