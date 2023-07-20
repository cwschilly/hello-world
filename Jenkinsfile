pipeline {
    agent any
    options {
        // This is required if you want to clean before build
        skipDefaultCheckout(true)
    }
    stages {
        stage('Build') {
            steps {
                timeout(time: 5, unit: 'HOURS') {
                    // Clean before build
                    cleanWs()
                    // We need to explicitly checkout from SCM here
                    checkout scm
                    echo 'Building...'
                    sh '''#!/bin/bash
                        git clone -c feature.manyFiles=true https://github.com/spack/spack.git
                        cd spack
                        . spack/share/spack/setup-env.sh
                        export JENKINS_HOME=/var/lib/jenkins/workspace/My-Pipeline_PR-3
                        spack env create trilinos-base
                        spack env activate trilinos-base
                        spack add git
                        spack add cmake
                        spack add ninja
                        spack add openmpi
                        spack add ccache
                        spack install
                        mkdir -p "${JENKINS_HOME}"/Trilinos-1
                        cd Trilinos-1
                        git clone https://github.com/NexGenAnalytics/Trilinos.git

                        export Trilinos_MPI_BUILD_DIR=/"${JENKINS_HOME}"/Trilinos-1/Trilinos-build
                        mkdir -p "${Trilinos_MPI_BUILD_DIR}"
                        cd "${Trilinos_MPI_BUILD_DIR}"

                        TRILINOS_SOURCE="/"${JENKINS_HOME}"/Trilinos-1/Trilinos"

                        cmake  \
                        -GNinja \
                        -D CMAKE_BUILD_TYPE=RELEASE \
                        -D CMAKE_CXX_STANDARD=17 \
                        -D Trilinos_ENABLE_DEBUG=ON \
                        -D CMAKE_VERBOSE_MAKEFILE=OFF \
                        -D TPL_ENABLE_MPI=ON \
                        -D MPI_BIN_DIR="$(which mpirun)" \
                        -D CMAKE_INSTALL_PREFIX=/"${JENKINS_HOME}"/Trilinos-1/Trilinos-install \
                        -D MPI_EXEC_MAX_NUMPROCS=8 \
                        -D MPI_C_COMPILER:FILEPATH="$(which mpicc)" \
                        -D MPI_CXX_COMPILER:FILEPATH="$(which mpicxx)" \
                        -D Trilinos_ENABLE_EXPLICIT_INSTANTIATION=ON \
                        -D Trilinos_ENABLE_ALL_OPTIONAL_PACKAGES=OFF \
                        -D Trilinos_ENABLE_TESTS=OFF \
                        -D Trilinos_ENABLE_EXAMPLES=OFF \
                        -D Trilinos_VERBOSE_CONFIGURE=OFF \
                        -D Tpetra_ENABLE_DEPRECATED_CODE=OFF \
                        -D Tpetra_INST_SERIAL=ON \
                        \
                        -D Trilinos_ENABLE_Zoltan2=ON \
                        -D Zoltan2_ENABLE_TESTS=ON \
                        -D Zoltan2_ENABLE_EXAMPLES=ON \
                        -D Kokkos_ENABLE_CUDA_LAMBDA=OFF \
                        \
                        -D Trilinos_ENABLE_Kokkos=ON \
                        \
                        -D BUILD_SHARED_LIBS:BOOL=ON \
                        -D CTEST_USE_LAUNCHERS:BOOL=TRUE  \
                        -D CMAKE_EXPORT_COMPILE_COMMANDS=1  \
                        ${TRILINOS_SOURCE}

                        ninja all -j4
                        ninja install
                        cd "${JENKINS_HOME}"/Trilinos-1/Trilinos-build
                        ctest -j8
                        '''
                    // githubNotify context: 'Jenkins: serial', description: 'OK',  status: 'SUCCESS'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
    post {
        // Clean after build
        always {
            cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true,
                    patterns: [[pattern: '.gitignore', type: 'INCLUDE'],
                               [pattern: '.propsfile', type: 'EXCLUDE']])
        }
    }
}