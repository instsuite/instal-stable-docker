# Instal-Stable dockerized
setting up INSTAL stable with peace of mind......


### How to use this repository
To use this, ensure that you have Docker installed on your laptop (no prior experience required as all steps are listed below)


### Getting Started

* Download this repository
* Extract the repository and place anywhere on your harddisk
* Using your terminal, `cd` into this repository
* Launch Docker and ensure its status is set to `running` before you proceed
* Set up `instal-clingo` using the steps below:
    * using your terminal, `cd` into the directory iclingo using the command `cd iclingo`
    * using your terminal, run the command `docker build . -t instal-clingo` [NOTE: This will take a while]
    * once the installation is complete, you should be able to use your terminal again
    * to ensure that the installation was successful, using your terminal, run the command `docker images`
    * look through the list until you find a repository named `instal-clingo` with the tag `latest`
    * using your terminal, navigate to the repository's root directory using the command `cd ..`
* Set up `instal-stable` using the steps below:
    * using your terminal `cd` into the directory istable using the command `cd istable`
    * using your termnial, run the command `docker build . -t instal-stable` [NOTE: This will take a while]
    * once the installation is complete, you should be able to use your terminal again
    * to ensure that the installation was successful, using your terminal, run the command `docker images` 
    * look trough the list until you find a repository named `instal-stable` with the tag `latest`
* All done... at this point you may use the terminal to `cd` anywhere you like....



### Using the docker images

To use the instal stable image, you would need to make a container. Rather than go into the details, I would discuss how to run the `greetings` examples and how things are slightly different yet similar to the normal way of running instal-stable


The example code for running greetings is contained in `instal-stable/examples/greeting/rude.sh` and it reads:
`../../instalsolve.py $* -i greeting.ial -f fact-greeting.iaf -d domain-greeting.idc -q polite.iaq -v`. The important bits here are that: 
* instalsolve is being run
* the files: greeting.ial, fact-greeting.idc, domain-greeting.idc and polite.iaq are being passed to it


Using this image you would need to do a few modifications as shown below (this is an example and you can modify it as you wish)

`docker run -v ~/Desktop/greetings:/workdir instal-stable solve $* -i /workdir/greeting.ial -f /workdir/fact-greeting.iaf -d /workdir/domain-greeting.idc -q /workdir/rude.iaq -v`

First of all, you need to note that docker containers are isolations so you need to bind a folder in your harddisk to one within the container to pass files across. In this case, the `greetings` folder on my desktop has been bound to a folder I am calling `workdir` in the instal-stable container.

`docker run` creates a container out of the specified image

hence `docker run -v ~/Desktop/greetings:/workdir instal-stable` says make a new instal-stable container and the files I place in the greetings folder on my desktop should be accessible to it under the folder workdir.
    
Now, the rest of the command: 
`solve $* -i /workdir/greeting.ial -f /workdir/fact-greeting.iaf -d /workdir/domain-greeting.idc -q /workdir/rude.iaq -v`
This is identical to what you have seen before. What it says is fire up instalsolve.py and pass all these to it. Note that I have appended `/workdir/` in front of all the file names. 



### How do I use other instal script?

Simple:

- Access `instalquery.py` by using the command: `docker run -v [path_to_files_on_your_pc]:[path_in_container] instal-stable query `
- Access `instalremote.py` by using the command: `docker run -v [path_to_files_on_your_pc]:[path_in_container] instal-stable remote `
- Access `instaltrace.py` by using the command: `docker run -v [path_to_files_on_your_pc]:[path_in_container] instal-stable trace `
- Access `instalsolve.py` by using the command: `docker run -v [path_to_files_on_your_pc]:[path_in_container] instal-stable solve `


Note: 
`[path_to_files_on_your_pc]` means the absolute path to where you have your iaq, idc and so on...
`[path_in_container]` means a folder within your container. It can be any random name. It does not need to already exist within the container.



### How do I export files?
If you make modifications to the base instal-stable code to enable it print out some logs (for instance), the ideal thing is to ensure these logs are saved to the bound directory. Looking at the example code above, [path_to_files_on_your_pc]is bound to [path_in_container]. So if we wanted to store logs to  the directory `/xyz`, we would have to bind a volume on or had disk to the directory `/xyz`within the container.

An alternative approach (though exhaustive) is to export the entire volume of the container to a tar.gz archive which you can then explore. see the `docker export -o [archive_name].tar.gz [instal-stable container_name]`





### Notice of bad practices....
Would be sorted out in the next iteration;

* having a `.tar.gz` of a repository within the folder iclingo is a bad practice. I couldn't get `wget` to stop failing so I refactored the Dockerfile to use that one instead.
* having the instal-stable files within istable is another bad practice. I should have created a nested repository which pulls a tagged version from the instal-stable repository. 




    
