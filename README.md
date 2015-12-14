# <a name="title"></a> Kitchen::Linode

A Test Kitchen Driver for Linode.

## <a name="requirements"></a> Requirements

**TODO:** document any software or library prerequisites that are required to
use this driver. Implement the `#verify_dependencies` method in your Driver
class to enforce these requirements in code, if possible.

## <a name="installation"></a> Installation and Setup

Install the gem file:
```
gem install kitchen-linode
```
Please read the [Driver usage][driver_usage] page for more details.

## <a name="config"></a> Configuration

For many of these, you can specify an ID number, a full name, or a partial name that will try to match something in the list but may not match exactly what you want.
```
LINODE_API_KEY      Linode API Key environment variable, default: nil
:username           ssh user name, default: 'root'
:password           password for user, default: randomly generated hash
:image              Linux distribution, default: "Debian 8.1"
:data_center        data center, default: "Atlanta"
:flavor             linode type/amount of RAM, default: "Linode 1024"
:payment_terms      if you happen to have legacy default: 1
:kernel             Linux kernel, default: "Latest 64 bit"
:private_key_path   Location of your private key file, default: "~/.ssh/id_rsa"
:public_key_path    Location of your public key file, default: "~/.ssh/id_rsa.pub"
:ssh_timeout        ssh timeout, default: 600 (seconds)
:sudo               use sudo, default: True
:port               ssh port, default: 22
```

## <a name="usage"></a> Usage

First, set your Linode API key in an environment variable:
```
$ export LINODE_API_KEY='myrandomkey123123213h123bh12'
```
Then, create a .kitchen.yml file:
```
---
driver:
  name: linode

provisioner:
  name: salt_solo
  formula: vim
  state_top:
    base:
      "*":
        - vim

platforms:
  - name: debian_jessie

suites:
  - name: default
```
then you're ready to run `kitchen test` or `kitchen converge`
```
$ kitchen test
```
If you want to create a second yaml config; one for using Vagrant locally but a second to use the Linode driver when run on your CI server, create a config with a name like `.kitchen-ci.yml`:
```
---
driver:
  name: linode

provisioner:
  name: salt_solo
  formula: vim
  state_top:
    base:
      "*":
        - vim

platforms:
  - name: debian_jessie

suites:
  - name: default
```
Then you can run the second config by changing the KITCHEN_YAML environment variable:
```
$ KITCHEN_YAML="./.kitchen-ci.yml" kitchen test
```
If you want to change any of the default settings, you can do it in the 'platforms' area:
```
...
platforms:
  - name: debian_jessie
    driver:
      flavor: 2048
      data_center: Dallas
      kernel: 4.0.2-x86_64-linode56
      image: Debian 7
...
```

### <a name="config-require-chef-omnibus"></a> require\_chef\_omnibus

Determines whether or not a Chef [Omnibus package][chef_omnibus_dl] will be
installed. There are several different behaviors available:

* `true` - the latest release will be installed. Subsequent converges
  will skip re-installing if chef is present.
* `latest` - the latest release will be installed. Subsequent converges
  will always re-install even if chef is present.
* `<VERSION_STRING>` (ex: `10.24.0`) - the desired version string will
  be passed the the install.sh script. Subsequent converges will skip if
  the installed version and the desired version match.
* `false` or `nil` - no chef is installed.

The default value is unset, or `nil`.

## <a name="development"></a> Development

* Source hosted at [GitHub][repo]
* Report issues/questions/feature requests on [GitHub Issues][issues]

Pull requests are very welcome! Make sure your patches are well tested.
Ideally create a topic branch for every separate change you make. For
example:

1. Fork the repo
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## <a name="authors"></a> Authors

Created and maintained by [Brett Taylor][author] (<btaylor@linode.com>)

## <a name="license"></a> License

Apache 2.0 (see [LICENSE][license])


[author]:           https://github.com/ssplatt
[issues]:           https://github.com/ssplatt/kitchen-linode/issues
[license]:          https://github.com/ssplatt/kitchen-linode/blob/master/LICENSE
[repo]:             https://github.com/ssplatt/kitchen-linode
[driver_usage]:     http://docs.kitchen-ci.org/drivers/usage
[chef_omnibus_dl]:  http://www.getchef.com/chef/install/
