#########################################################################################
# HOWTO/TUTORIAL COMPILE A RPM SPEC FILE
# TARGET:  classification-banner that is challenging to setup on RHEL8/Python3.x
# SRC:     https://src.fedoraproject.org/forks/kenyon/rpms/classification-banner.git
# REF:     all references are located down at the bottom of this HOWTO/TUTORIAL
# LICENSE: GPLv2+
#########################################################################################


# rpmdevtools: install it

    yum -y groupinstall development

    yum -y install rpmdevtools

    # yum -y install rpmlint                                  # for spec file errors


# rpmdev-setuptree:  build rpmbuild setup tree

    rpmdev-setuptree

    # mkdir -p ~/rpmbuild/{BUILD,RPMS,SOURCES,SPECS,SRPMS}    # same as above; haven't verified
    # echo '%_topdir %(echo $HOME)/rpmbuild' > ~/.rpmmacros


# cd to target directory

    cd ~/rpmbuild/SPECS/

# download specfile and other source code; at fedoraproject git repo

    git clone https://src.fedoraproject.org/forks/kenyon/rpms/classification-banner.git

    cd classification-banner/


# updated changelog, since it was originally for fedora and now it's rhel8.4; it's under "%changelog"

    # start snip (don't copy)

        grep -B 1 -A 2 -i 'Chris' classification-banner.spec

        grep -B 1 -A 2 -i 'Chris' classification-banner.spec
        %changelog
        * Fri Sep 10 2021 Chris H <github.com/rac3rx> - 1.7.0-11
          - Rebuilt for rhel8.4

    # end snip (don't copy)

    rpmlint classification-banner.spec                        # verify we didn't introduce errors
    
    
# download source files and dependencies plus build source

    spectool -g -R classification-banner.spec                 # downloads source0 in specfile; ie main program under "Name"

    sudo dnf builddep classification-banner.spec              # downloads BuildRequires & Requires (specfile's dependencies)

    rpmbuild -bs classification-banner.spec                   # builds source; ~/rpmbuild/SRPMS/classification-banner-1.7.0-11.el8.src.rpm

    rpmbuild -bb classification-banner.spec                   # builds binary; ~/rpmbuild/RPMS/noarch/classification-banner-1.7.0-10.el8.noarch.rpm

    # rpmbuild -ba classification-banner.spec                 # builds both


# install rpm
    sudo yum -y localinstall ~/rpmbuild/RPMS/noarch/classification-banner-1.7.0-11.el8.noarch.rpm


# REF:
# https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html-single/rpm_packaging_guide/index#working-with-spec-files
# https://wiki.centos.org/HowTos/SetupRpmBuildEnvironment
# https://www.redhat.com/sysadmin/create-rpm-package
# https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/11/html-single/rpm_packaging_guide/index#working-with-spec-files
# https://stackoverflow.com/questions/3634650/can-i-use-rpm-to-expand-the-macros-in-a-specfile
# https://stackoverflow.com/questions/33177450/how-do-i-get-rpmbuild-to-download-all-of-the-sources-for-a-particular-spechttps://stackoverflow.com/questions/13227162/automatically-install-build-dependencies-prior-to-building-an-rpm-package
