# RUN PKG

## Using Docker
```bash
cd ~/rsl_traversability/docker
##image build
docker compose -f docker-compose-gui-nvidia.yaml build
##create container
docker compose -f docker-compose-gui-nvidia.yaml up -d
docker exec -it docker-rsl_travel-1 /bin/bash

##in container
cd ~/rsl_travel_ws/
catkin make
source devel/setup.bash

## elevation_launch ##
roslaunch elevation_mapping_demos real_world.launch
## travel_launch ##
roslaunch traversability_estimation traversability_estimation.launch
```
