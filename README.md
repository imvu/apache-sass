# apache-sass

This is an Apache plugin that uses libsass to compile SCSS to CSS on the fly.

## How to compile
### Installing dependencies

This package was built at IMVU by first building an upstream debian package
of libsass 1.0.1 and placing it into our apt repo.  At IMVU,
we chose to pull down debian package from http://mentors.debian.net/package/libsass.

```
dget -x http://mentors.debian.net/debian/pool/main/libs/libsass/libsass_1.0.1-1.dsc
```

The latest source in this pacakge is available at
https://github.com/hcatlin/libsass.

Install this dependency with "apt-get" if you are at IMVU use this or
"dpkg" if you built your own debian packge.

```
sudo apt-get install libsass-dev
```

or

```
sudo dpkg -i libsass-dev_1.0.1-1_amd64.deb
```

### Compiling

Create a subdirectory for the clone this repo. Then run debuild -us -uc.
If you are prompted to install dependencies, install them.  If you are
building this for IMVU, you must use pbuilder to build this package in
a pristine environment.

```
# mkdir apache-sass-workspace
# cd apache-sass-workspace
# git clone git@github.com:imvu/apache-sass
# cd apache-sass
# debuild -us -uc
# cd ..
# ls -l
total 28
drwxrwxr-x 5 jschedler jschedler 4096 2014-02-05 10:55 apache-sass/
-rw-r--r-- 1 jschedler jschedler 2857 2014-02-04 15:23 libapache2-mod-sass_1.0.2_amd64.build
-rw-r--r-- 1 jschedler jschedler 1254 2014-02-04 15:23 libapache2-mod-sass_1.0.2_amd64.changes
-rw-r--r-- 1 jschedler jschedler 4968 2014-02-04 15:23 libapache2-mod-sass_1.0.2_amd64.deb
-rw-r--r-- 1 jschedler jschedler  701 2014-02-04 15:23 libapache2-mod-sass_1.0.2.dsc
-rw-r--r-- 1 jschedler jschedler 4092 2014-02-04 15:23 libapache2-mod-sass_1.0.2.tar.gz

```

## Installation and Configuration
### At IMVU
Just run sudo cfagent.

### For Others
You will need to install the libsass0 package followed by the libapache2-mod-sass package.  Most likely you have already built the libsass0 debian package if you got as far as building the libapaceh2-mod-sass package in this repo.

```
sudo dpkg -i libsass0_1.0.1-1imvu1_amd64.deb
sudo dpkg -i libapache2-mod-sass_1.0.2_amd64.deb
```

Next, you will want to configure the apache module.  By creating a sass.conf file in /etc/apache2/mods-available.  It should look something like this.

```
AddHandler sass .css
SassIncludePaths /home/cit/imvu/website
```
Then, create the appropriate symlinks to enable the module, and restart apache
```

cd /etc/apache2/mods-enabled
sudo ln -s ../mods-available/sass.load
sudo ln -s ../mods-available/sass.conf
sudo /etc/init.d/apache2 restart
```

_NOTE /etc/apache2/mods-available/sass.load is part of the libapache2-mods-sass debian package._
