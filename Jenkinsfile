pipeline {
    agent {
        docker {
            image 'pierrpebay/vt-tv:latest'
        }
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh '''sudo apt-get install python3'''
        //         timeout(time: 5, unit: 'HOURS') {
        //             // install Python and Spack
        //             sh '''
        //                 apt install python3
        //                 git clone -c feature.manyFiles=true https://github.com/spack/spack.git
        //                 . spack/share/spack/setup-env.sh
        //                 spack env create trilinos-build
        //                 spack env activate trilinos-build
        //                '''
        //             // install other dependencies
        //             sh '''spack add git
        //                 spack add cmake
        //                 spack add ninja
        //                 spack add openmpi
        //                 spack add ccache
        //                 spack install
        //                 source /home/calebschilly/spack/share/spack/setup-env.sh
        //                '''
        //             // clone Trilinos
        //             sh ''' git clone https://github.com/NexGenAnalytics/Trilinos.git
        //                '''
        //             // build Trilinos
        //             sh '''#!/bin/bash
        //                 export Trilinos_MPI_BUILD_DIR=/home/Trilinos-build
        //                 mkdir -p "${Trilinos_MPI_BUILD_DIR}"
        //                 cd "${Trilinos_MPI_BUILD_DIR}"

        //                 TRILINOS_SOURCE="/home/Trilinos"

        //                 cmake  \
        //                 -GNinja \
        //                 -D CMAKE_BUILD_TYPE=DEBUG \
        //                 -D CMAKE_CXX_STANDARD=17 \
        //                 -D Trilinos_ENABLE_DEBUG=ON \
        //                 -D CMAKE_VERBOSE_MAKEFILE=OFF \
        //                 -D TPL_ENABLE_MPI=ON \
        //                 -D MPI_BIN_DIR="$(which mpirun)" \
        //                 -D CMAKE_INSTALL_PREFIX=/home/Trilinos-install \
        //                 -D MPI_EXEC_MAX_NUMPROCS=8 \
        //                 -D MPI_C_COMPILER:FILEPATH="$(which mpicc)" \
        //                 -D MPI_CXX_COMPILER:FILEPATH="$(which mpicxx)" \
        //                 -D Trilinos_ENABLE_EXPLICIT_INSTANTIATION=ON \
        //                 -D Trilinos_ENABLE_ALL_OPTIONAL_PACKAGES=OFF \
        //                 -D Trilinos_ENABLE_TESTS=OFF \
        //                 -D Trilinos_ENABLE_EXAMPLES=OFF \
        //                 -D Trilinos_VERBOSE_CONFIGURE=OFF \
        //                 -D Tpetra_ENABLE_DEPRECATED_CODE=OFF \
        //                 -D Tpetra_INST_SERIAL=ON \
        //                 \
        //                 -D Trilinos_ENABLE_Zoltan2=ON \
        //                 -D Zoltan2_ENABLE_TESTS=ON \
        //                 -D Zoltan2_ENABLE_EXAMPLES=ON \
        //                 -D Kokkos_ENABLE_CUDA_LAMBDA=OFF \
        //                 \
        //                 -D Trilinos_ENABLE_Kokkos=ON \
        //                 \
        //                 -D BUILD_SHARED_LIBS:BOOL=ON \
        //                 -D CTEST_USE_LAUNCHERS:BOOL=TRUE  \
        //                 -D CMAKE_EXPORT_COMPILE_COMMANDS=1  \
        //                 ${TRILINOS_SOURCE}

        //                 ninja all -j4
        //                 ninja install
        //                 cd home/Trilinos-build
        //                 ctest -j8 --verbose
        //                 '''
        //             githubNotify context: 'Jenkins: serial', description: 'OK',  status: 'SUCCESS'
        //         }
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
}