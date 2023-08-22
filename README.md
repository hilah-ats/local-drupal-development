# Local Drupal Development
## Setup

1. Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop/).

2. Open Terminal and pull the latest Docker image for [Drupal](https://hub.docker.com/_/drupal) (or specify a version).
	``` docker pull drupal:version```
	
3. Pull the latest Docker image for [MySQL](https://hub.docker.com/_/mysql/) (or specifify a version compatible with the version of Drupal you are using).
	``` docker pull mysql:version```
	
4. Create the following folder structure in a directory on your local filesystem.
	``` 
	/my-directory
		- /sites 
		- /profiles
		- /modules
		- /themes
	```
	
5. Download a .zip of the latest version of [Drupal](https://www.drupal.org/project/drupal/releases) (or the version of Drupal you previously specified).

6. Copy the contents of the ``` /sites ``` folder from the downloaded .zip into the ```/sites``` folder in the directory you created on your local filesystem.

7.  Copy ```/my-directory/sites/default.settings.php``` and rename the copy to ```/my-directory/sites/settings.php```.

8. Open Docker Desktop and use the UI to create a new container from the pulled Drupal image. Under "Optional settings" define the following values and then click "Run".
	``` 
	Container name: my-drupal-container
	Ports:
		Host port: 8080
	Volumes:
		/my-directory/sites: /var/www/html/sites
		/my-directory/profiles: /var/www/html/profiles
		/my-directory/modules: /var/www/html/modules
		/my-directory/themes: /var/www/html/themes
	``` 
	
9. Use the Docker Desktop UI to create a new container from the pulled MySQL image. Under "Optional settings" define the following values and then click "Run".
	``` 
	Container name: my-mysql-container	
	Environment variables:
		MYSQL_DATABASE: drupal
		MYSQL_USER: user
		MYSQL_PASSWORD: password
		MYSQL_ROOT_PASSWORD: password
	``` 
	
10. Return to Terminal and run ```docker network create my-network``` to create a new container network. 

11. Run the following commands to connect both of the containers you created to your new container network.
	```
	docker network connect my-network my-drupal-container
	docker network connect my-network my-mysql-container
	```
	
12. You should now be able to visit ```localhost:8080``` and begin Drupal's initial setup.

13. When prompted during setup to enter the database information, use the values you defined previously for ```MYSQL_DATABASE```, ```MYSQL_USER```, and ```MYSQL_PASSWORD```. Additionally, under "Advanced Options", set the host to ```my-mysql-container```.

14. Congratulations! You have now installed Drupal and are ready for local development.
