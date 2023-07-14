# [Dockefile] instal-stable

`Dockerfile` and `dependencies` to setup `instal-stable` image. 


#### Requirements

- `Docker`: Latest version & should be running
- Functional internet connection 

#### Building the image 

* Download this repository and `cd` into the downloaded directory
* Build image using teh command `docker build -t instal-stable .`  (this should build the image with the tag `instal-stable`.. this will take a while)


#### Using the docker images / creating a container

To use the `instal-stable` image, you would need to make a container. 
To build a container, run the command, use the `docker run` command as discussed in the section below.

##### - Container requirements:

* `shared volume`: As `instal` may require some files (`iac`, `ial` & `iaq` files, based on usecase), when creating a container, you would need to share volume between your system and the container to make those files available

Rather than go into the details, I would discuss how to run the `greetings` examples and how things are slightly different yet similar to the normal way of running instal-stable

##### - Building a container that runs the greetings example:

The example code for running greetings is contained in `instal-stable/examples/greeting/rude.sh` and it reads:
`../../instalsolve.py $* -i greeting.ial -f fact-greeting.iaf -d domain-greeting.idc -q polite.iaq -v`. The important bits here are that: 
* `instalsolve` is being run.
* the files: `greeting.ial`, `fact-greeting.idc`, `domain-greeting.idc` and `polite.iaq` are being passed to it.


Using this image you would need to make a few modifications as shown below (this is an example and you can modify it as you wish)

`docker run -v ~/Desktop/greetings:/workdir instal-stable solve $* -i /workdir/greeting.ial -f /workdir/fact-greeting.iaf -d /workdir/domain-greeting.idc -q /workdir/rude.iaq -v`

First of all, you need to note that docker containers are isolations so you need to bind a folder in your harddisk to one within the container to pass files across. In this case, the `greetings` folder on my desktop has been bound to a folder I am calling `workdir` in the instal-stable container.

`docker run` creates a container out of the specified image (`instal-stable` image in this case)

Hence, `docker run -v ~/Desktop/greetings:/workdir instal-stable` says make a new `instal-stable` container and the files I place in the `greetings` folder on my desktop should be accessible to the container under the folder `workdir`.
    
Now, the rest of the command: 
`solve $* -i /workdir/greeting.ial -f /workdir/fact-greeting.iaf -d /workdir/domain-greeting.idc -q /workdir/rude.iaq -v`
This is identical to what you have seen before. What it says is fire up `instalsolve.py` and pass all these files to it. Note that I have appended `/workdir/` in front of all the file names. 



#### How do I use other instal script?

Simple,
- Access `instalquery.py` by using the command: `docker run -v [path_to_files_on_your_pc]:[path_in_container] instal-stable query `
- Access `instalremote.py` by using the command: `docker run -v [path_to_files_on_your_pc]:[path_in_container] instal-stable remote `
- Access `instaltrace.py` by using the command: `docker run -v [path_to_files_on_your_pc]:[path_in_container] instal-stable trace `
- Access `instalsolve.py` by using the command: `docker run -v [path_to_files_on_your_pc]:[path_in_container] instal-stable solve `


Note: 
`[path_to_files_on_your_pc]` means the absolute path to where you have your iaq, idc and so on...
`[path_in_container]` means a folder within your container. It can be any random name. It does not need to already exist within the container.



#### How do I export files?
If you make modifications to the base instal-stable code to enable it print out some logs (for instance), the ideal thing is to ensure these logs are saved to the bound directory. Looking at the example code above, [path_to_files_on_your_pc]is bound to [path_in_container]. So if we wanted to store logs to  the directory `/xyz`, we would have to bind a volume on or had disk to the directory `/xyz`within the container.

An alternative approach (though exhaustive) is to export the entire volume of the container to a tar.gz archive which you can then explore. see the `docker export -o [archive_name].tar.gz [instal-stable container_name]`

#### Need assistance?
If something doesn't work as expected, kindly use the `issues` tab to report this.


    
