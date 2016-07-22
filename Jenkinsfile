#!groovy

node('SLAVE') {
    stage 'Checkout'
        checkout scm
        sh '''git submodule update --init'''

    stage 'JavaScript Testing'
        sh '''./autotest-js.sh'''
        step([$class: 'JUnitResultArchiver', testResults: 'tests/autotest-results-js.xml'])

    stage 'PHPUnit'
        sh '''
        export NOCOVERAGE=1
        unset USEDOCKER
        phpenv local 7.0
        ./autotest.sh sqlite
        phpenv local 5.4
        ./autotest.sh mysql
        phpenv local 5.6
        ./autotest.sh pgsql
        phpenv local 5.5
        ./autotest.sh oci
        '''
        step([$class: 'JUnitResultArchiver', testResults: 'tests/autotest-results-sqlite.xml'])
        step([$class: 'JUnitResultArchiver', testResults: 'tests/autotest-results-mysql.xml'])
        step([$class: 'JUnitResultArchiver', testResults: 'tests/autotest-results-oci.xml'])
        step([$class: 'JUnitResultArchiver', testResults: 'tests/autotest-results-pgsql.xml'])

    stage 'Files External Testing'
        sh '''phpenv local 7.0
        export NOCOVERAGE=1
        unset USEDOCKER
        ./autotest-external.sh sqlite webdav-ownCloud
        ./autotest-external.sh sqlite smb-silvershell
        ./autotest-external.sh sqlite swift-ceph
        '''
        step([$class: 'JUnitResultArchiver', testResults: 'tests/autotest-external-results-sqlite.xml'])
        step([$class: 'JUnitResultArchiver', testResults: 'tests/autotest-external-results-sqlite-webdav-ownCloud.xml'])
        step([$class: 'JUnitResultArchiver', testResults: 'tests/autotest-external-results-sqlite-smb-silvershell.xml'])
        step([$class: 'JUnitResultArchiver', testResults: 'tests/autotest-external-results-sqlite-swift-ceph.xml'])
}

