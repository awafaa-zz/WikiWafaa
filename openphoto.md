#OpenPhoto/Trovebox#
OpenPhoto is what became of Trovebox, there used to be a hosted version but this closed down in 2014/2015.

##Installation##
First we need to add the PHP Extension repository. This is needed as there are several php5 packages required that are not provided by the OSS repo:
`zypper ar http://download.opensuse.org/repositories/server:/php:/extensions/openSUSE_Factory/ php:extensions`

Now that the correct repositories are in place, install the dependencies:
` zypper in php5 apache2 apache2-mod_php5 exiftran mariadb php5-APC php5-curl php5-imagick php5-mcrypt php5-oauth`

Pull the latest source code via git
`git clone https://github.com/photo/frontend.git openphoto`
