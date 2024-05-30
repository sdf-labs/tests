# Official SDF Tests Library

This SDF library contains the standard tests available in the default namespace for SDF data tests. As such, tests defined in this workspace do not need to be prefixed with the workspace name, they can be referenced directly. Here's an example:

```yaml
columns:
  - name: a
    tests:
      - expect: unique()
      - expect: not_null()   
```

*Note: SDF is still < v1, as such certain scenarios may result in unexpected behavior. Please follow the [contributing guidelines](./CONTRIBUTING.md) and create an issue in this repo if you find any bugs or issues.*

For an in-depth guide on how to use SDF tests, please see the Tests section of [our official docs](https://docs.sdf.com/guide/data-quality/tests)

