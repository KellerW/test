# Movie Manager Application Example
## Pre-requirements 

### For Local Development 
To compile and run the application on your local machine without using containers, you need to install the following dependencies:

- [Google Test Framework](https://github.com/google/googletest): A testing framework for C++ code.
- [Crow Framework](https://github.com/CrowCpp/Crow): A lightweight and fast C++ micro web framework.

Follow these steps to install the dependencies:

1. Installing Compiler, Build Tools and Visual Code 
```
bash
sudo apt update
sudo apt install build-essential cmake git libasio-dev
sudo apt install python3
sudo snap install code --classic
```
2. Conan Install steps
```
Install Python (3.7+) 
pip install --user -U conan
source ~/.bashrc
conan 
conan profile detect --force
vim conan profile path default
Set Conan for C++ 17. 
```
#### STEPS 2 and 4 involve manual compilation.
3. Goolge the Test framework :
```
mkdir /home/gtest  
cd /home/gtest          
git clone https://github.com/google/googletest.git 
cd /home/gtest/googletest 
mkdir build 
cd build 
cmake .. 
make -j8
make install
```
4. The Crow Framework
```
mkdir /home/${USER}/crow 
cd /home/${USER}/crow          
git clone https://github.com/CrowCpp/Crow.git 
cd /home/${USER}/crow/Crow 
mkdir build 
cd build 
cmake -DCROW_BUILD_EXAMPLES=OFF -DCROW_BUILD_TESTS=OFF .. 
make -j8 
make install
```
## To build and run the Application
```
mkdir /home/gitroot       
cd /home/gitroot         
git clone https://github.com/KellerW/movietheathermanager.git 
cd movietheathermanager 
rm -r build  
mkdir build 
cd build
conan install .. -s build_type=Release -s compiler.cppstd=17 --output-folder=. --build missing \
cmake .. or cmake -DCMAKE_BUILD_TYPE=Debug  ..
make -j8 
```
Go to the folder /build and run the application Movie manager: 
```
cd /build
./MovieManager
```
## Running in Containers 
### Installing Docker Dependencies
 - First, ensure Docker is installed on your system. If not, you can install it by following these steps [here](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04)

# For other distributions or macOS/Windows, refer to the official Docker documentation.
### Building the Container 
Once Docker is installed, you can build the container image for your application:
```
docker build -t moviemanager .
```
### Running the Container
Ensure Docker is installed and the user has permissions to access Docker without sudo. Follow the steps provided in the reference here.

After successfully building the container image, you can run it using the following command:
```
docker container run -it -p 8888:8888 moviemanager
```
This command starts a container based on the moviemanager image, allowing access to the application running on port 8888. Adjust the port mapping as needed for your application.
#### The home Screen:

![Screenshot from 2024-04-21 04-12-15](https://github.com/KellerW/movietheathermanager/assets/2254792/0684042a-7a7e-4f68-b963-8722bfcbfc93)


#### Dont Run with Sudo permission, add the user and run without sudo following the 
the reference: 
[here](https://betterstack.com/community/questions/how-to-fix-permission-denied-error-when-connecting-to-docker/)

### Docker Utils
#### Stop all the containers
```
docker stop $(docker ps -a -q)
```
#### Remove all the containers
```
docker rm $(docker ps -a -q)
```
#### To run a disposable new container, you can simply attach a tty and standard input:
```
docker run --rm -it --entrypoint bash <image-name-or-id>
```
#### Or to prevent the above container from being disposed, run it without --rm.
#### Or to enter a running container, use exec instead:
```
docker exec -it <container-name-or-id> bash
```
#### Clean Up Containers
```
docker container prune
```
#### Run Container in root user 
```
docker exec -u root -t -i container_id /bin/bash
```
[reference](https://docs.docker.com/reference/cli/docker/container/rm/)
# Accessing the Application
Once the application is running, access it by navigating to `http://localhost:8888` in your web browser.

## To build the documentation:
 1. Create a directory named `docs` in the root project folder.
 2. Execute the following command:
 
 ```
 doxygen
 ```
 
### All documentation will be generated and located in `docs/html`. 

## To build and Run the tests
```
rm -rf build
mkdir build
cd build && conan install .. -s build_type=Release -s compiler.cppstd=17 --output-folder=. --build missing
cd build && cmake -DENABLE_UNIT_TEST=ON -DCMAKE_BUILD_TYPE=Release .. && make -j16

```
## Navigate to `build/tests` and run:
```
ctest -R ModelTests
```
## All tests and builds can be executed in the `container` with root user access. The project is located at `/home/gitroot/moviemanager`.

### Install and configure Conan before use.

## Folder Structure
- `src`: Contains the source code for the Movie Manager Application.
- `tests`: Contains unit tests for the application.
- `docs`: Contains documentation files.

## Contributing
Contributions are welcome! Please open an issue or submit a pull request for any enhancements or bug fixes.
## License
This project is licensed under the [MIT License](LICENSE).
