#!/bin/bash

# To update the version shipped in this quickstart, bump this variable:
#
install_version="4.0"

# Download and install WordPress

install_dir=${OPENSHIFT_BUILD_DEPENDENCIES_DIR}${install_version}

# Used in this script only
current_version_dir=${OPENSHIFT_DATA_DIR}current

#
# If WordPress is already installed in the current gear, there
# is nothing to build :-)
#
[ -d "${current_version_dir}" ] && exit 0

mkdir -p $install_dir

pushd ${install_dir} >/dev/null
curl -Ls http://wordpress.org/wordpress-${install_version}.tar.gz > wordpress-${install_version}.tar.gz

# Verify the sources
#
tarball_md5=$(md5sum wordpress-${install_version}.tar.gz | cut -d ' ' -f 1)
wordpress_md5=$(curl -Ls http://wordpress.org/wordpress-${install_version}.tar.gz.md5)

if [ "${tarball_md5}" != "${wordpress_md5}" ]; then
  echo "ERROR: WordPress ${install_version} MD5 checksum mismatch."
  exit 1;
fi

# Install WordPress
#
tar --strip-components=1 -xzf wordpress-${install_version}.tar.gz
rm -rf wordpress-${install_version}.tar.gz
echo $install_version > ${OPENSHIFT_BUILD_DEPENDENCIES_DIR}.current_version

popd >/dev/null
