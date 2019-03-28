INSTALLATION INSTRUCTIONS OF AGORA-EXO PACKAGE

*Clone AGoRA-exo repositories
git clone https://github.com/midasama3124/agora
git clone https://github.com/midasama3124/epos_library

#TODO Create meta-package with both packages embedded

*Access EPOS library package and install main source
cd ~/catkin_ws/src/epos_library
sudo bash ./install.sh (ROOT privileges)

*Compile both packages within catkin workspace
cd ~/catkin_ws
catkin_make install

Add at the end of the last instruction the following command where applicable
either [-DCATKIN-WHITESPACES = "agora"]
or [-DCATKIN-WHITESPACES = "epos_library"]
