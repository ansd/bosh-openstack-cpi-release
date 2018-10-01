The `auto-update` pipeline automatically updates gems and bosh packages before creating a pull request in this repo.

The `update-packages` job is triggered whenever there is a new bosh package version (ruby, rubygems, bundler, libyaml) available.
For each package a separate [dynamic-metalink-resource](https://github.com/dpb587/dynamic-metalink-resource) checks for new versions.

In this concourse resource, a `version_check` script with custom logic must be provided which runs periodically in the `check` step of the concourse resource.
For example, for ruby this script parses the http://cache.ruby-lang.org/pub/ruby/index.txt website and outputs a list of all semvers.
For rubygems, bundler, and libyaml git remote tags are listed in order to identify all semvers.

Also,  a `metalink_get` script with custom logic must be provided which runs in the `in` script of the concourse resource.
This script defines how a [metalink](https://tools.ietf.org/html/rfc5854) file gets created based on the latest version determinded in the `check` script.

The `update-gems` job is triggered periodically. It calls `bundle update` which updates all gems of the [Gemfile](https://github.com/cloudfoundry-incubator/bosh-openstack-cpi-release/blob/master/src/bosh_openstack_cpi/Gemfile) ignoring the previously installed gems specified in the [Gemfile.lock](https://github.com/cloudfoundry-incubator/bosh-openstack-cpi-release/blob/master/src/bosh_openstack_cpi/Gemfile.lock).
