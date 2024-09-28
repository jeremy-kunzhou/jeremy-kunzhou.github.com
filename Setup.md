# Development

## if using rvm, target ruby version is 2.7.7
```
rvm use
```
init environment
```
./script/bootstrap
```
dev
```
./script/server
```
build
```
./script/build

```

# Trouble shooting
## jekyll Permission denied @ rb_sysope
delete the _site and try again
```
sudo rm -rf ./_site
```

https://github.com/jekyll/jekyll/issues/7170
[Using jekyll with bundler](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/)
```
bundle config set --local path 'vendor/bundle'
```