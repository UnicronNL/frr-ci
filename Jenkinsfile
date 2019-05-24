pipeline {
  agent none
  stages {
    stage('build-package') {
      parallel {
        stage('Build package amd64') {
          agent {
            docker {
              label 'jessie-amd64'
              args '--privileged --sysctl net.ipv6.conf.lo.disable_ipv6=0 -e GOSU_UID=1006 -e GOSU_GID=1006 -v /tmp:/tmp'
              image 'higebu/vyos-build:current'
            }

          }
          steps {
            sh '''#!/bin/bash
git clone --single-branch --branch master https://github.com/FRRouting/frr.git $BUILD_NUMBER
cd $BUILD_NUMBER
git clean -dfx
git reset --hard
sudo apt-get -o Acquire::Check-Valid-Until=false update
echo "Preparing the build"
./tools/tarsource.sh -V
echo "Building the Debian package"
dpkg-buildpackage -us -uc -Ppkg.frr.rtrlib
mkdir -p /tmp/$GIT_BRANCH/packages/script
mv ../*.deb /tmp/$GIT_BRANCH/packages/'''
          }
        }
        stage('Build package armhf') {
          agent {
            docker {
              label 'jessie-amd64'
              image 'vyos-build-armhf:current'
              args '--privileged --sysctl net.ipv6.conf.lo.disable_ipv6=0 -e GOSU_UID=1006 -e GOSU_GID=1006 -v /tmp:/tmp'
            }

          }
          steps {
            sh '''#!/bin/bash
git clone --single-branch --branch master https://github.com/FRRouting/frr.git $BUILD_NUMBER
cd $BUILD_NUMBER
git clean -dfx
git reset --hard
sudo apt-get -o Acquire::Check-Valid-Until=false update
echo "Preparing the build"
./tools/tarsource.sh -V
echo "Building the Debian package"
dpkg-buildpackage -us -uc -Ppkg.frr.rtrlib
mkdir -p /tmp/$GIT_BRANCH/packages/script
mv ../*.deb /tmp/$GIT_BRANCH/packages/'''
          }
        }
        stage('Build package arm64') {
          agent {
            docker {
              label 'jessie-amd64'
              args '--privileged --sysctl net.ipv6.conf.lo.disable_ipv6=0 -e GOSU_UID=1006 -e GOSU_GID=1006 -v /tmp:/tmp'
              image 'vyos-build-arm64:current'
            }

          }
          steps {
            sh '''#!/bin/bash
git clone --single-branch --branch master https://github.com/FRRouting/frr.git $BUILD_NUMBER
cd $BUILD_NUMBER
git clean -dfx
git reset --hard
sudo apt-get -o Acquire::Check-Valid-Until=false update
echo "Preparing the build"
./tools/tarsource.sh -V
echo "Building the Debian package"
dpkg-buildpackage -us -uc -Ppkg.frr.rtrlib
mkdir -p /tmp/$GIT_BRANCH/packages/script
mv ../*.deb /tmp/$GIT_BRANCH/packages/'''
          }
        }
      }
    }
    stage('Deploy packages') {
      agent {
        node {
          label 'jessie-amd64'
        }

      }
      steps {
        sh '''#!/bin/bash
cd /tmp/$GIT_BRANCH/packages/script
/var/lib/vyos-build/pkg-build.sh $GIT_BRANCH'''
      }
    }
    stage('Cleanup') {
      parallel {
        stage('Cleanup amd64') {
          agent {
            node {
              label 'jessie-amd64'
            }

          }
          steps {
            cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true, cleanupMatrixParent: true, deleteDirs: true, disableDeferredWipeout: true)
          }
        }
        stage('Cleanup armhf') {
          agent {
            node {
              label 'jessie-amd64'
            }

          }
          steps {
            cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true, cleanupMatrixParent: true, deleteDirs: true, disableDeferredWipeout: true)
          }
        }
        stage('Cleanup arm64') {
          agent {
            node {
              label 'jessie-amd64'
            }

          }
          steps {
            cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true, cleanupMatrixParent: true, deleteDirs: true, disableDeferredWipeout: true)
          }
        }
      }
    }
  }
}
