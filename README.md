# camera-control-app with Docker Compose for ARM arch

Make sure that Docker is installed on your system and port 8081 is free

Navigate to the directory:
```sh
cd SonyARM-init
```

We will setup a crontab to extract images from the docker container periodically and delete them from the container to save space inside the container
Install crontab if it is not already installed
```sh
sudo apt-get update
sudo apt-get install cron
```

Open the crontab 
```sh
crontab -e
```

When you open the crontab for the first time, cron will ask you to select a method to edit, enter '1' and then hit enter to select nano as your editor
```sh
1
```

Add the following line to the end of the file to the file to extract the images every minute. The frequency of extraction can be changed as needed
```sh
* * * * * /home/user/extract.sh
```

To extract images every hour, add the following line:
```sh
0 * * * * /home/user/extract.sh
```
Hit 'CTRL+X', 'y', 'Enter' to save the file in the nano editor

Replace /home/user with the path where the script exists on your host filesystem. To get the exact path, go to the directory with the script from your terminal and enter:
```sh
pwd
```

For example, the output on this system (where the code was tested) is:
```sh
/home/enyuare/camera-control-app/SonyARM-init
```

To select a location to store images, open extract.sh and change the 'destination' to your desired folder to save images


Build with Docker:
```sh
docker compose build
```

Power cycle the camera. Run the Docker Container and wait for 5s to initialize the camera 
```sh
docker compose up -d
```
To click a picture
```sh
cd app
python3 commandClick.py
```
The camera image will be stored inside the docker container. To see the image inside the docker container, you can ssh into the container in a new terminal (or existing) with the following command:
```sh
docker exec -it camera-control-app /bin/bash
```

To see the newly captured image:
```sh
ls
```

After you click images, go to your destination directory to observe pics every minute. Once the images appears in the desired directory, they are deleted from the container
