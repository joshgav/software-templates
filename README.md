## Prerequisites

Some Backstage software templates for use with custom Backstage scaffolder actions.

- [Kubernetes Actions](https://github.com/janus-idp/backstage-plugins/tree/main/plugins/kubernetes-actions)
- [Backstage Plugins for AWS](https://github.com/awslabs/backstage-plugins-for-aws)

## To build as dynamic plugins for RHDH

- Build the [Kubernetes Actions plugin](https://github.com/janus-idp/backstage-plugins/tree/main/plugins/kubernetes-actions) into a dynamic plugin.
    - Follow the instructions at <https://github.com/gashcrumb/dynamic-plugins-getting-started>
    - Specifically, run `yarn run export-dynamic` and `01-stage-dynamic-plugins.sh`
    - Note the SHA512 output, you'll need it later
- Build [the AWS Backstage plugins](https://github.com/awslabs/backstage-plugins-for-aws) repo into a dynamic plugin, specifically `plugins/core/scaffolder-actions`.
    - Again, follow the instructions at <https://github.com/gashcrumb/dynamic-plugins-getting-started>
    - Specifically run `yarn run export-dynamic` and `01-stage-dynamic-plugins.sh`
    - Note the SHA512 output, you'll need it later
    - Some commands may need to be executed at the root of the repo
- Find the built plugins at `./deploy` in the root or child directories.
- Gather the built plugins together in one `./deploy` directory and run `02-create-plugin-registry.sh` or `03-update-plugin-registry.sh`
    - In OpenShift this will spin up an httpd container/pod serving the built plugins in the current namespace.
- Refer to the new plugins in the dynamic plugins list as follows:

```yaml
    - package: 'http://plugin-registry:8080/aws-aws-core-plugin-for-backstage-scaffolder-actions-dynamic-0.3.1.tgz'
      disabled: false
      integrity: 'sha512-+lvQlESXIb9kXtUVfMy3RijLOH9HtWnZz4VUfZxHwu2mg8Dsk3fJ0jXozGdqRqJXY9VOeCcLqWYwxfZqiffSSw=='
    - package: 'http://plugin-registry:8080/janus-idp-backstage-scaffolder-backend-module-kubernetes-dynamic-2.3.0.tgz'
      integrity: 'sha512-JGY0+dUxx91zy/1E5u9QUNJFuUg8lSeoMHgbHYbg1S3zB31ZweWkijE476yb+WkpdAgJcXbO6SCurBe37pbmqQ=='
      disabled: false
```
