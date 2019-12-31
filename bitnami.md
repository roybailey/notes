### Quick links

[Bitnami Applicances](https://bitnami.com/)

### Wordpress and clone steps

-	download bitnami wordpress VM Applicance
-	import into VirtualBox
-	start bitnami wordpress (username=`bitnami` password=`bitnami`\)
-	move to `/opt/bitnami/apps/wordpress`
-	permission `sudo chmod -R 777 htdocs` (so wordpress can install plugins and overwrite files etc.)
-	launch browser to `ifconfig` IP address
-	login to `/wp-admin` (username=`user` password=`bitnami`\)
-	install plugin `File Manager` for direct FTP capability
-	goto `File Manager` and upload into root folder the xcloner backup tar file
-	install plugin `XCloner`
-	copy `wp-content/plugins/xcloner.../restore/*` to root `htdocs` folder
-	goto `/XCloner.php`
	-	mysql server `localhost`
	-	mysql username `root`
	-	mysql password `bitnami`
	-	mysql database `bitnami_wordpress`
