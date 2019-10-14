Install K8s On a Local System.

Minikube on a local system.


Minikube bundles all of these different components into a single image.
Providing us a pre-configured single node kubernetes cluster.

Link to kubeadm installation instructions:  https://kubernetes.io/docs/setup/independent/install-kubeadm/

We need to verify the mac and product_uuid are unique for every node.


	+===============================COMMANDS============================+	
	|	MAC ADD --> ip link    /   ifconfig -a			    |
	|	product_uuid --> sudo cat /sys/class/dmi/id/product_uuid    |
	+===================================================================+

Steps to ceate a cluster :

	/~~~~~~~~~~~~~~~~~~~~~~~~~~~~STEPS~~~~~~~~~~~~~~~~~~~~~~~~~~~\
	
	/	First Step :					     \
			Identifi the Master and Worker Nodes.
	/	Second Step :					     \
			Insatll Docker on them /\/\
	/	Third Step :					     \
			Install the kubeadm ( kube admin tool )
	/	Fourth Step :					     \
			Initialize the master of our kluster.
	/	Fifth Step :					     \
			Create a POD Network.
	/	Sixth Step :					     \
			Join the worker Nodes to the Master.
	/~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~\


Now we need to make sure our Nodes all will have static IP.

	We need 2 two interfases 
				\1/ For Internet ( No static IP address ) .
				\2/ For lockal lan between our master and nodes.

There are two ways to assign Static Ip Address.
	
  +=======================================================================================+
  |										       	  |	
  |				 +===========================================+	       	  |
  |	~~~			 |	       ( The static IP add  )        |	       	  |
  |	\1/ Type the command --> | ifconfig enp0s 192.168.56.2		     |	       	  |
  |      ~  			 | We need to be sudo user --> sudo su       |	       	  |
  |	    			 +===========================================+	       	  |
  |	    									       	  |
  |	    This ip add will be temporary as soon as boot our node.		       	  |
  |	    When we shutbown our node the IP will lost.   			       	  |
  +=======================================================================================+	

			        [- Permanent IP assign-] 
  
  +===================================================================================================================+
  														
  																				      
  	     																			      
 ~~~		     cat /etc/network/interfaces														      
  \2/ Type command --> vim /etc/network/interfaces  ( On sudo user )												      
   ~		     																		      
   You will see --> # interfaces(5) file used by ifup(8) and ifdown(8)									      
  		     auto lo															
                     iface lo inet loopback													
  			Or something like that.														
  		Have only one interface for loopback.												
  		Now we need to insert one more interface ( For our Static IP ).										  
  		      																	
  We need to whrite something like these :  												
  
  # interfaces(5) file used by ifup(8) and ifdown(8)										      			
  auto lo															     
  iface lo inet loopback													     
  # Configure enp0s8 interface												     
  auto enp0s8 ( Automatically enables / turn on these interface in our case --> enp0s8 when the operating system boot up)          
  iface enp0s8 inet static ( We say this particular interface --> enp0s8 will have a static IP  )				     
  address 192.168.56.2 ( You need to specify the static IP you want to give to your device )       			     
  netmask 255.255.255.0 ( You need to specify the network mask you want to give to your device )			      
  					 												
  		      															
  																	
+===================================================================================================================+																
  																	
  

  				    [-SWAP-]
   
+===================================================================================================================+
  																
  																
  	You MUST disable swap in order for the kubelet to work properly.							
 																
  	You need to do two things to disable / switch off swap 									
  																
  	- The first thing you need to do is to execute this command --> swapoff -a						
  																
  	- Second step, you need to go to the fstab file.  ( every text editor you like e.x. : vi, vim, sublime ... )	 
  			So to go there you need to execute the command --> vim /etc/fstab 				
  															
        														
                            Third you will se something like :                   						
        	# /etc/fstab: static file system information.								
      		#													
        	# Use 'blkid' to print the universally unique identifier for a						
        	# device; this may be used with UUID= as a more robust way to name devices				
        		# that works even if disks are added and removed. See fstab(5).						
        		#													
        		# <file system> <mount point>   <type>  <options>       <dump>  <pass>					
        		# /was on												
        		# / was on /dev/sda1 during installation								
        		UUID=23688f67-7abf-4cf1-ad6c-fce1295e8ba6 /               ext4    errors=remount-ro 0       1		
        		/swapfile                                 none            swap    sw              0       0		
        															
        															
        		Now comment the last line for the swap you can put comment with ( # )                                   
                                                                                        					
 +===================================================================================================================+
  

  
Now we need to Intall the Docker to do that we need to go to this link --> [Docker page]

(https://docs.docker.com/install/linux/docker-ce/ubuntu/)


   It is for ubuntu linux  
								 
		We need to execute all these commands at all of our nodes.
		\/ \/ \/ \/ \/ \/ \/ \/ \/ \/ \/ \/ \/ \/ \/ \/ \/ \/ \/
				Set up the repository :
											
         		+==================+
			|  Update the apt  |
		   ~~~  +==================+
		   \1/   $ sudo apt-get update
		    ~
		    +====================================================+
		    |  Install packages to allow apt to use a repository |
		    +====================================================+
	            ~~~
		    \2/  $ sudo apt-get install \
    		     ~		apt-transport-https \
			        ca-certificates \
				curl \
	    			gnupg-agent \
         			software-properties-common
		+==================================================+
		|	Add Docker’s official GPG key  		   |
		+==================================================+		
		      ~~~		
		      \3/  $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
		       ~
			 +============================================================+
			 | Use the following command to set up the stable repository. |
			 +============================================================+	
			~~~
			\3/  $ sudo add-apt-repository \
			 ~   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
			     $(lsb_release -cs) \
			     stable"
   
			+================================================+
			|	Update the apt package index	         |
			+================================================+
			 ~~~
			 \4/ $ sudo apt-get update  
			  ~ 
							
	     		+================================================+
			|Install the latest version of Docker Engine     |
			| it is optionaly				 |
		        +================================================+
			 ~~~
			 \5/ $ apt-cache madison docker-ce
			  ~ 
			 +================================================+
			 | See the List of the versions are available     |
			 |in your repo				          |
			 +================================================+
                         ~~~
			 \6/ $ sudo apt-get install docker-ce=<VERSION_STRING>
			  ~ 
			  +================================================+
			  | <VERSION_STRING> <-- put the docker version    |
			  |you want to install			           |
			  +================================================+
                          ~~~
			  \7/ $ sudo docker run hello-world
			   ~  $ docker version
			     +================================================+
			     | Test if docker is intall properly              |
			     |                        			      |
		             +================================================+


				**NOW YOU NEED TO INSTALL**
+===================================================================================================================+
	You will install these packages on all of your machines:

    	kubeadm: the command to bootstrap the cluster.
          
	kubelet: the component that runs on all of the machines in your cluster and does things like starting pods and containers.

	kubectl: the command line util to talk to your cluster.	

		                                   1st Step
+===================================================================================================================+
		For Ubuntu, Debian

		apt-get update && apt-get install -y apt-transport-https curl
		curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
		cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
		deb https://apt.kubernetes.io/ kubernetes-xenial main
		EOF
		apt-get update
		apt-get install -y kubelet kubeadm kubectl
		apt-mark hold kubelet kubeadm kubectl

						    2nd Step ( Create our cluster)										
+===================================================================================================================+

		 Now create our cluster and initialize our master for cluster


						( A ) Master initialization
+===================================================================================================================+								
										
								
	 You must install a pod network add-on so that your pods can communicate with each other.
	 You can install only one pod network per cluster.
	 You can install a pod network add-on with the following command on the control-plane node / master node:
	 kubectl apply -f <add-on.yaml>
								
	 Flannel is a pod network.
	 							
	 For flannel to work correctly, you must pass --pod-network-cidr=10.244.0.0/16 to kubeadm init.

	 Kubeadm uses the network interface associated with the default gateway to set the advertise address 
	 for this particular control-plane node’s API server. To use a different network interface, 
	 specify the --apiserver-advertise-address=<ip-address> argument to kubeadm init.

	  So the command you need execute is : 
																		  <the master's node IP address>

	$sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=<ip-address>
	
	                                                 ( B ) Insert nodes to claster
+===================================================================================================================+

	   After all of these you should take a message like :

								
[init] Using Kubernetes version: vX.Y.Z
[preflight] Running pre-flight checks
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Activating the kubelet service
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [kubeadm-cp localhost] and IPs [10.138.0.4 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [kubeadm-cp localhost] and IPs [10.138.0.4 127.0.0.1 ::1]
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [kubeadm-cp kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 10.138.0.4]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[apiclient] All control plane components are healthy after 31.501735 seconds
[uploadconfig] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-X.Y" in namespace kube-system with the configuration for the kubelets in the cluster
[patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "kubeadm-cp" as an annotation
[mark-control-plane] Marking the node kubeadm-cp as control-plane by adding the label "node-role.kubernetes.io/master=''"
[mark-control-plane] Marking the node kubeadm-cp as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[bootstrap-token] Using token: <token>
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstraptoken] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstraptoken] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstraptoken] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstraptoken] creating the "cluster-info" ConfigMap in the "kube-public" namespace
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  /docs/concepts/cluster-administration/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join <control-plane-host>:<control-plane-port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>
								

You need to excecute : 
  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
								

Now go to all tha nodes and copy paste the token --> 
kubeadm join <control-plane-host>:<control-plane-port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>
It mast be like  --> kubeadm join 192.168.56.2:6443 --token t3w926.ep2cll3hfuje7utp --discovery-token-ca-cert-hash sha256:9a594c03c05ced648f1dfbb92d6da35e8eec8e04c8115a9908e0d2ba5bef808c

+===================================================================================================================+ 

										Now you have the cluster.


										To test if everithing work ok
+===================================================================================================================+
								1) $kubectl get pods --all-namespaces 
									To see all the processes at you cluster.
								1) $kubectl get nodes
									To see all cluster nodes.

									
+================================================THE END=============================================================+













