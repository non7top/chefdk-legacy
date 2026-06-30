mini project
create docker image using chefdk rpm files
chefdk-1.6.1-1.el7.x86_64.rpm
chefdk-1.6.1-1.el6.x86_64.rpm

use centos6 and centos7 appropriately
the files are at https://packages.chef.io/files/stable/chefdk/1.6.1/el/6/, but can;t downloadthem
name the images chefdk, sign, upload as artifacts

not sure of its better to uploadfiles here as directlyinto the repo, or push them as relases.
add gh action to buildimages
also push the rpms as a part of the release files
