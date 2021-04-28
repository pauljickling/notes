# RPM

rpm is the package manager for the RHEL OS.

`rpm -ivh {package name}-{version number}.{architecture}.rpm` installs a package of the specified version number for the specified architecture

`rpm -e {package name}` uninstalls the package

`rpm -Uvh {package name}-{version number}.{architecture}.rpm` upgrades the package to specified version and architecture

`rpm -Fvh {package name}-{version number}.{architecture}.rpm` freshens the package i.e. only updates existing packages. If no existing package exists, no update will take place.

`rpm -Fvh *.rpm` updates all existing packages.

`rpm -q {package name}` queries info about a package such as version number. The `-i` flag provides additional info.

`rpm -Vf {path/to/package}` Verifies the installed package with a checksum

`rpm -Va` verifies all packages

`rpm -Vp {package name}-{version number}.{architecture}.rpm` verifies installed package against RPM package.

`rpm -K --nosignature {rpm file}` md5 check
