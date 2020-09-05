# Google Cloud Fundamentals: Getting Started with Compute Engine

### Creating virtual machine using command line

In GCP console, on the top right toolbar, click the Open Cloud Shell button. Then click **continue**

To check the list of zones available in a region assigned by Qwiklabs. Execute the command below:

```sh
$ gcloud compute zones list | grep us-central1
```
In this case, us-central1 is the region assigned by Qwiklabs.

Choose a zone among the listed zones that is different from the zone that has been used before.

To set a specific zone, use **compute gcloud config set compute/zone** followed by the zone you chose.

#### Create virtual machine my-vm-1 in zone us-central1-a

```sh
$ gcloud config set compute/zone us-central1-a
```
In this case, **us-central1-a** is the selected zone.

To create the virtual machine called **my-vm-1**, use the command below.
```sh
$ gcloud compute instances create "my-vm-1" --machine-type "n1-standard-1" \
--image-project "debian-cloud" \ --image "debian-9-stretch-v20190213" \ --subnet "default"
```

#### Create virtual machine my-vm-2 in zone us-central1-b
```sh
$ gcloud config set compute/zone us-central1-b
```
In this case, **us-central1-b** is the selected zone.

To create the virtual machine called **my-vm-2**, use the command below.
```sh
$ gcloud compute instances create "my-vm-2" --machine-type "n1-standard-1" \
--image-project "debian-cloud" \ --image "debian-9-stretch-v20190213" \ --subnet "default"
```

Execute **exit** command to close the cloud shell.

### Connect between virtual machines
In the navigation menu, click **Compute Engine > VM instances.**

You will see the two VM instances you created, each in a different zone.

Notice that the Internal IP addresses of the instances share the first three bytes in common. They reside on the same subnet in their Google Cloud VPC even though they are in different zones.

To open a command prompt on the my-vm-2 instance, click SSH in its row in the VM instances list.

Let's confirm **my-vm-2** van reach **my-vm-1** over the network using **ping**
```sh
$ ping my-vm-1
```
Press Ctrl+C to stop the ping command.

ssh into my-vm-1 from my-vm-2 by executing the command below.

**ssh my-vm-1**

Type **yes** to confirm you want to continue connecting to the host

On **my-vm-1** command prompt, install nginx web server using the command below
```sh
$ sudo apt-get install nginx-light -y
```
Use the **nano** text editor to add a custom message to the home page of the web server using the command below.

```sh
$ sudo nano /var/www/html/index.nginx-debian.html
```
Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace **YOUR_NAME** with your name:
```sh
$ Hi from YOUR_NAME
```
Press **Ctrl+O** and then press Enter to save your edited file, and then press **Ctrl+X** to exit the nano text editor.

To confirm that the web server is serving your new page, execute below on the command prompt on my-vm-1:
```sh
$ curl http://localhost/
```
The response will be the HTML source of the web server's home page, including your line of custom text.

Type **exit** command to exit **my-vm-1** command prompt.

To confirm that **my-vm-2** can reach the web server on **my-vm-1**, at the command prompt on **my-vm-2**, execute this command:

```sh
$ curl http://my-vm-1/
```
The response will again be the HTML source of the web server's home page, including your line of custom text.

Copy the External IP address for **my-vm-1** and paste it into the address bar of a new browser tab. You will see your web server's home page, including your custom text.