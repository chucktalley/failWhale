# failWhale

README for posting a static maintenance page at the web server

Using a maintenance script, traffic can be rerouted to a specific, static maintenance page without the need to reboot either the web server service or the deployed applications. This function utilizes two configuration files and one shell script and is intended to be incorporated into an Oracle HTTP Server (OHS) web server.

Files referenced
	- mod_wl_ohs.conf – OHS configuration file containing the virtual host directives
		Note: if using Apache HTTP Server, this configuration can be placed in httpd.conf
	- splash.html – Static web page that users will be directed to during the maintenance window
	- script – Shell script used to post and remove the static web page for the maintenance window

Implementation
- mod_wl_ohs.conf
	- Once OHS is installed, edit mod_wl_ohs.conf, typically located at <MIDDLEWARE_HOME>/Oracle_WT1/instances/<OPMN_INSTANCE>/config/OHS/<OHS_INSTANCE>/mod_wl_ohs.conf
	- Choose a local listen port for the virtual host to use on the web server, and replace all instances of <PORT-A> with that port [ll. 3-5]
	- Replace <WEB_SERVER_HOST> with the IP or hostname that the user will use to access the online environment [l. 7]
	- Replace ‘<HOST-1>:<PORT-1>’ with the host and port of the application instance on the application server.  If there are multiple, clustered instances, separate them by commas with no additional spaces [l. 22]
	- If there are particular IPs that need to access ABMS while the batch window page is up, replace the placeholder IP addresses with the desired IPs. [l. 11]
- splash.html
	- Place the HTML file that will be used as the static page during the batch window in the /www directory on the OHS web server. [filename]
	- The contents of the HTML file need not relate to the contents of the sample file
	- Make sure the file name matches the html file name from mod_wl_ohs.conf [ll. 12-13]
	- Make sure that the /www directory has at least read and execute permissions for the public permissions.  The HTML file must be public-readable.
- script
	- Place the file in /etc/init.d (assumes RHEL OS)
	- Make sure that the file is executable by whatever user is executing the batch scheduler
	- Execute by running 
		# /sbin/service script {start|stop|status}


Author:
Chuck Talley
chucktalley89@gmail.com