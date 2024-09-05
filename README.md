# Supported Ruby Versions Action

This GitHub Action generates a list of Ruby versions. The output is intended for use as a Matrix strategy by other workflows.

## Usage

By default, the action will output all Ruby's `Major.Minor` version pairs >= 3.1.

```
name: Unit Tests
on: push
jobs:
  ruby-versions:
    runs-on: ubuntu-latest
    outputs:
      versions: ${{ steps.versions.outputs.supported_versions }}
    steps:
    - id: versions
      uses: actions/supported_ruby_versions@v1

  tests:
    name: Unit Tests
    runs-on: ubuntu-latest
    needs: ruby-versions
    strategy:
      matrix:
        ruby: ${{ fromJson(needs.ruby-versions.outputs.versions) }} # [3.1, 3.2, 3.3]
    steps:
    - uses: actions/checkout@v4
    - uses: ruby/setup-ruby@v1
    - run: bundle exec rspec
```

You can also use the RubyGems requirement syntax to customize the versions returned by the action.

```
    steps:
    - uses: actions/supported_ruby_versions@v1
      with:
        version_requirement: "~> 3.0"
```
