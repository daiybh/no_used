
# Install Conan-server  and start

    pip install conan-server


modify C:\Users\daili\.conan_server\server.conf

line:62  remove the "#"  

let 
[write_permissions]
*/*@*/*: *

# Create "mypkg" package

    mkdir mypkg && cd mypkg

    conan new cmake_lib -d name=mypkg -d version=1.0

    conan create . -s build_type=Debug

    conan create . -s build_type=Release



# upload "mypkg" package to conan-server

check if already add local conan-server


    conan remte list


if no:

//conan remote add "<name_for later use>" "<remote conan Server URL>"

    conan remote add my_local_server http://localhost:9300



    conan search "mypkg" -r=my_local_server

will be failed "ERROR: Recipe 'mypkg' not found"

    conan upload mypkg/1.0 -r=my_local_server

search again

    conan search "mypkg" -r=my_local_server

sucess.


# clean local Cache

    remove local cache

    conan remove "mypkg" --confim

    conan search "mypkg"

Found 1 pkg/version recipes matching mypkg in my_local_server
conancenter
  ERROR: Recipe 'mypkg' not found
my_local_server
  mypkg
    mypkg/1.0
	
	

# use "mypkg" from local "Conan-server"	

goto the code folder

    conan install . --output-folder=build --build=missing -s build_type=Debug
    conan install . --output-folder=build --build=missing -s build_type=Release

    cmake --preset conan-default

    cmake --build --preset="conan-release"
    
    cmake --build --preset="conan-debug"


