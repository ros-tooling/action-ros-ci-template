# ROS2 GitHub Action CI Template

Use this template to create a ROS2 package repository with automatic CI.

Waiting for a buildfarmer to kickoff a build for your PR is time consuming.
With this action, you can immediately get linting and tests feedback.
Minimize the number of builds you need to kickoff and catch bugs earlier!

For more information on using GitHub action workflows, see the
[Official GitHub Workflow Documentation].

## Usage

Click on the green `Use this Template` to create your new repository.

### Linting

This action supports the linters in the [ament/ament_lint] repo.
Linter names are based off of ament's naming convention `ament_<linter-name>`.
For example, to use `ament_uncrustify`, set `uncrustify` as your linter.

An example job for `copyright` linting.

```yaml
ament_copyright:
  name: ament_copyright
  runs-on: ubuntu-18.04
    steps:
    - uses: actions/checkout@v1
    - uses: ros-tooling/setup-ros2@0.0.11
    - uses: ros-tooling/action-ros2-lint@0.0.5
      with:
        linter: copyright
        package-name: <your-package-name>
```

For multiple linters, use a `matrix`:

```yaml
# Linters applicable to C++ packages
ament_lint_cpp:
  # Give each job a different name for better feedback
  name: ament_${{ matrix.linter }}
  runs-on: ubuntu-18.04
  strategy:
    # We want all linters to run even if one fails
    fail-fast: false 
    matrix:
      linter: [cppcheck, cpplint, uncrustify]
    steps:
    - uses: actions/checkout@v1
    - uses: ros-tooling/setup-ros2@0.0.11
    - uses: ros-tooling/action-ros2-lint@0.0.5
      with:
        linter: ${{ matrix.linter }}
        package-name: <your-package-name>
```

See [`.github/workflows/lint.yml`] for an example linting workflow.

**Note:** The only unsupported linter at this time is CMake.

[Official GitHub Workflow Documentation]: https://help.github.com/en/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions
[ament/ament_lint]: https://github.com/ament/ament_lint/tree/eloquent
[`.github/workflows/lint.yml`]: .github/workflows/lint.yml
