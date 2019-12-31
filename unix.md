# UNIX
--

##### delete all files in current folder tree

`find . -name "*.log" -delete`

`find . -name "*.log" -exec rm {} \;`

`find . -name "*.log" -exec rm -rf {} \;`

`find . -type d -name "target" -exec rm -rf {} \;`

##### OSX hibernate mode

`sudo pmset -a hibernatemode 3`

`pmset -g | grep hibernate`


command | description
--------|------------
`tar -zxvf druid-services-*-bin.tar.gz` | extracting tar files

