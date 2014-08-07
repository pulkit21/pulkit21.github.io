---
layout: post
title: " Installing Ruby 2.0.0 on an Amazon Linux AMI"
date: 2014-08-07 11:38:07 +0530
comments: true
categories: 
---

Install a ruby 2.0.0 on an Amazon AMI. This is just a quick update to get version 2.0.0 up. We used the current 64 bits Amazon AMI. Note that the complete procedure might take a few hours on a Amazon Micro instance. Deleting the old version of Ruby also removes the package aws-amitools-ec2. As long as you don’t need to create your own AMIs with this instance, you will be fine.
Here is the procedure, as usual log in as ec2-user and perform the following from the shell:

Login as root
```sh
sudo su -
cd
```
Remove the old version of Ruby
```sh
yum remove ruby
```
Install the dependency packages
```sh
yum groupinstall 'Development Tools'
yum groupinstall development-libs
yum install libffi-devel
yum install libyaml-devel
yum update 
```
After intalling the dependency packages download and  install **Ruby  2.0.0**
```sh
wget ftp://ftp.ruby-lang.org/pub/ruby/2.0/ruby-2.0.0-p195.tar.gz
tar xzvf ruby-2.0.0-p195.tar.gz
cd ruby-2.0.0-p195
./configure
make
make install
make clean
gem update --system
exit
ruby --version (this should give you the newly installed version)
```
That’s it! Have fun with Ruby!

  