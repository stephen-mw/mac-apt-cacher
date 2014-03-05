# mac-apt-cacher settings
These settings will allow you to run `apt-cacher-ng` on your Mac without
opening port 80 to everyone on the internet and become some zombie relay.

## Why use these settings?
The default settings for `apt-cacher-ng` listens on all interfaces. This
is very bad and effectively turns your computer into an open web proxy.
It also doesn't allow you to adjust the normal acng settings (such as
expiration time).

By default, these settings will only listen on 127.0.0.1 and will cache
for 30 days.

## Procedure
```
brew install apt-cacher-ng
```
Brew may suggest some commands in its caveats.  Don't execute them.

Instead, do this:
```
git clone git@github.com:stephendotexe/mac-apt-cacher.git
mv mac-apt-cacher /usr/local/etc/apt-cacher-ng
sudo cp -fv /usr/local/opt/apt-cacher-ng/*.plist /Library/LaunchDaemons
sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.apt-cacher-ng.plist
```
Now `apt-cacher-ng` is running and it will restart whenever your Mac reboots.

# Integrating this with Vagrant
Now that you've got a running apt-cacher on your mac, integrating it with
Vagrant is easy. Simply drop the following line into
`/etc/apt/apt.conf.d/02proxy` after your host boots up. You can do this
automatically using Vagrant's built-in provisioning system.

```
...
$api_bootstrap = <<BOOTSTRAP
echo 'Acquire::http { proxy "http://10.0.2.2:3142"; };' > /etc/apt/apt.conf.d/02proxy
apt-get update
apt-get upgrade
BOOTSTRAP
...
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.provision "shell", inline: $api_bootstrap
...
```

After the first install things should go _considerably_ faster for you. You
will also prevent a lot of traffic from flowing through your public network.
This is especially important if you have multiple people at the same time
using Vagrant.
