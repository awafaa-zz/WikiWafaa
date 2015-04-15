#RPM Packaging#

##Macros##

* %{_bindir} `/usr/bin`

* %{_libdir} `/usr/lib` or `/usr/lib64`

* %{_datadir} `/usr/share`

* %{_sysconfdir} `/etc`

* %{_localstatedir} `/var`

List of Macros can be found in `/usr/lib/rpm/macros` and `/usr/lib/rpm/suse_macros/etc/rpm/macro*`

##Licensing##
All licenses need to be in the form of SPDX License variables, which can be found [here][1].
[1]: http://spdx.org/licenses/ "Approved SPDX licenses"

##Useful links##

* [Build Service Tutorial][2]

* [Build Service Commandline interface][3]

* [How-to package for multiple distros][4]

* [Geeko (SUSE & openSUSE) packaging conventions][5]

* [Geeko RPM Macros][6]

* [Build Service Collaboration][7]

* [Building Debian packages on the OBS][8]

* [Fedora Guide to packaging][9]

[2]: http://en.opensuse.org/Build_Service_Tutorial
[3]: http://www.opensuse.org/Build_Service/CLI
[4]: http://en.opensuse.org/Build_Service/cross_distribution_package_how_to
[5]: http://en.opensuse.org/Packaging/SUSE_Package_Conventions
[6]: http://en.opensuse.org/SUSE_Package_Conventions/RPM_Macros
[7]: http://en.opensuse.org/Build_Service/Collaboration
[8]: http://en.opensuse.org/Build_Service/Deb_builds
[9]: http://docs.fedoraproject.org/drafts/rpm-guide-en/

##Package Groups##
Suitable package groups to use in the spec file can be found in either [RPM Groups][10] or [Desktop Menu category list][11]
[10]: http://en.opensuse.org/SUSE_Package_Conventions/RPM_Groups
[11]: http://en.opensuse.org/SUSE_Package_Conventions/Desktop_Menu#9.4._Category_List

##Linking and diffing packages##

To diff: `osc rdiff --oldprj Moblin:2.2 openSUSE:11.2 $package`

To link: `osc rdelete Moblin:2.2 $package; osc linkpac -c openSUSE:11.2 $package Moblin:2.2`

##Patching using quilt##
This is lifted from _darix's_ blog which I have forgotten the link to.

`osc co yourproject/yourpackage`

`cd yourproject/yourpackage`

`quilt setup -v *spec` There are occaisions where you will need to comment out some macros

`cd yourpackage-*/`

`quilt push -a` This applies any existing patches

`quilt new yourpackage-version_fixbuild.patch`

`quilt edit src/foo.c`

`quilt refresh`

`foo-fixbuild.patch` will automatically be created in the parent directory if you work on a package which doesnt have a patch yet. You have to remember to
copy the patch from the patch directory to your package directory.

Normally I rerun `quilt setup` once I have an initial patch. There is also quilt pop so you can remove patches from your working copy, but the docs have a lot more informations about this.

##Package name differences##
To find a package in openSUSE when you have the Fedora name:
`zypper se $FIRST_BIT_OF_NAME | grep $SECOND_BIT_OF_NAME`

e.g. in Fedora the package is `expat-devel` so run `zypper se expat | grep devel`