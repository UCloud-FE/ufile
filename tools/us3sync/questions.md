#Frequently asked questions
##After the master is deployed, the browser cannot see the interface
###Cause of problem
The Linux Firewall shields the corresponding network ports.
###Solutions
Check whether the web service listening port of the machine firewall is turned on, turn off the firewall or set the IP white list on the port.
###Cause of problem
Because us3sync uses a self signed certificate, it is not recognized by chrome and other browsers.
###Solutions
Enter thisisunsafe on the chrome error page.
##After the worker is deployed, the task being migrated after the task is started is always displayed as 0
###Cause of problem
The failure to start the worker node is usually caused by the network failure between the worker node and the master node.
###Solutions
Check the network connectivity through Ping, check whether the IP address of the worker node is configured correctly, and ensure the network connectivity between the master and the worker node.
##There is an error in the task. In the exported task log, the error is "client.timeout or context cancellation"
###Cause of problem
The worker node tried to upload / download files, but the request timed out due to network reasons or the server did not respond, causing this error.
###Solutions
Check the network connectivity between the endpoint and the worker machine through Ping or MTR. If the network is normal, you can click the retry button to try to retransmit the failed files, or you can consider increasing the timeout of the endpoint.
##In the exported task log, the error status code is 403
###Cause of problem
The public and private keys configured on endpoint do not have permission to upload, download or list
###Solutions
Please update the permission of the public and private keys and confirm that they have the permission to upload, download and list