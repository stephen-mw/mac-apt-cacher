# mac-apt-cacher settings
These settings will allow you to run apt-cacher-ng on your mac without opening port 80 to everyone on the internet and become some zombie relay.

## Why use these settings?
The default settings for apt-cacher-ng listens on all interfaces. This is very bad and effectively turns your computer into an open web proxy. It also doesn't allow you to adjust the normal acng settings (such as expiration time).

By default, these settings will only listen on 127.0.0.1 and will cache for 30 days.

## Procedure
First, brew install apt-cacher-ng
```
brew install apt-cacher-ng
```

Next, clone this repo and copy the contents over to /etc/apt-cacher-ng
```
git clone git@github.com:stephendotexe/mac-apt-cacher.git
mv mac-apt-cacher /etc/apt-cacher-ng
```

Now you can start apt-cacher-ng using the supplied settings
```
sudo apt-cacher-ng -c /etc/apt-cacher-ng
```
