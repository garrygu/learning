Dockerfile contains instructions to be performed when an image is built.  
[Dockerfile reference](https://docs.docker.com/engine/reference/builder/)

## Dockerfile Format
>\# Comment  
>INSTRUCTION arguments

## Dockerfile Commands (Instructions)
- **FROM**  *Set base image*  
	- 	Usage: FROM '<image>'
    	- 	Example:
    	>	FROM ubuntu:latest  
    	- 	A valid Dockerfile must have **FROM** as its first non-comment instruction
    	- 	**FROM** can appears multiple times within a single Dockerfile in order to create multiple images
- **MAINTAINER**  *Set author of iamge to be generated*
    - Usage: MAINTAINER <name>
- **RUN**  *Execute any commands in a new layer on top of the current image*
    - **Usage**: RUN <command>
    - Example: 
    -       RUN /bin/bash -c 'source $HOME/.bashrc; echo $HOME'
        or
    -       RUN /bin/bash -c 'source $HOME/.bashrc;\    # use \ to continue on a new line
            echo $HOME'
- **ADD**  *Copy new files, directories or remote file URLs and adds them to the filesystem of the container*
    - Usage: ADD <src>... <dest>
    - Example:
    -     ADD hom* /mydir/        # adds all files starting with "hom"
          ADD hom?.txt /mydir/    # ? is replaced with any single character, e.g., "home.txt"
          ADD test relativeDir/   # adds "test" to `WORKDIR`/relativeDir/
          ADD test /absoluteDir/  # adds "test" to /absoluteDir/
- **COPY**  *Copy new files or directories and adds them to the filesystem of the container *
    - Usage: COPY <src>... <dest>
    - Example:
    -       COPY hom* /mydir/        # adds all files starting with "hom"
            COPY hom?.txt /mydir/    # ? is replaced with any single character, e.g., "home.txt" 
            COPY test relativeDir/   # adds "test" to `WORKDIR`/relativeDir/
            COPY test /absoluteDir/  # adds "test" to /absoluteDir/
- **EXPOSE**  *Inform Docker that the container listens on the specified network ports at runtime*
    - Usage: EXPOSE <port> [<port>...]
    - **EXPOSE** does not make the ports of the container accessible to the host. Must use either the -p flag to publish a range of ports or the -P flag to publish all of the exposed ports.
- **CMD**  *Provide defaults for an executing container*
    - Usage: 
    -       CMD ["executable","param1","param2"]    (exec form, this is the preferred form)
            CMD ["param1","param2"]                 (as default parameters to ENTRYPOINT)
            CMD command param1 param2               (shell form)
    - Example:
    -       FROM ubuntu
            CMD echo "This is a test." | wc -   # shell form, execute in /bin/sh -c
            or
            CMD ["/usr/bin/wc","--help"]        # must express the command as a JSON array to run the <command> without a shell
    -   If used more than once, the last CMD in the Dokcerfile will be launched (good for one process per container rule)
- **ENTRYPOINT**  *Allow you to configure a container that will run as an executable*  
For example, start nginx with its default content, listening on port 80:  
    -       docker run -i -t --rm -p 80:80 nginx
    - Usage:
    -       ENTRYPOINT ["executable", "param1", "param2"]   (exec form, preferred)
            ENTRYPOINT command param1 param2                (shell form)
    - Example:  
            **Exec form ENTRYPOINT example**
    -       # can use the exec form of ENTRYPOINT to set fairly stable default commands and arguments and then use either form of CMD to set additional defaults that are more likely to be changed:
            FROM ubuntu
            ENTRYPOINT ["top", "-b"]
            CMD ["-c"]
    -       # run Apache in the foreground (i.e., as PID 1):
            FROM debian:stable
            RUN apt-get update && apt-get install -y --force-yes apache2
            EXPOSE 80 443
            VOLUME ["/var/www", "/var/log/apache2", "/etc/apache2"]
            ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
        **Shell form ENTRYPOINT example**
    -       FROM ubuntu
            ENTRYPOINT exec top -b  
            # start with exec to ensure that docker stop will signal any long running ENTRYPOINT executable correctly
    - Command line arguments to docker run <image> will be appended after all elements in an exec form ENTRYPOINT, and will override all elements specified using CMD. This allows arguments to be passed to the entry point, i.e., docker run <image> -d will pass the -d argument to the entry point.
    - Can override the ENTRYPOINT instruction using the docker run --entrypoint flag
    - The shell form prevents any CMD or run command line arguments from being used (but the executable will not receive a SIGTERM from docker stop <container>)
    - Only the last ENTRYPOINT instruction in the Dockerfile will have an effect
- **VOLUME**  *Create a mount point and mark it as holding externally mounted volumes from native host or other containers*
    - The value can be a JSON array, VOLUME ["/var/log/"], or a plain string with multiple arguments, such as VOLUME /var/log or VOLUME /var/log /var/db.
    - Example:   
    -       FROM ubuntu
            RUN mkdir /myvol
            RUN echo "hello world" > /myvol/greeting
            VOLUME /myvol
- **USER**  *Set the user name or UID when running the image and for any RUN, CMD and ENTRYPOINT that follow it in the Dockerfile*
    - Usage: USER daemon 
- **WORKDIR**  *Set the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD that follow it in the Dockerfile*
    - Usage: WORKDIR /path/to/workdir
    - If the WORKDIR doesn’t exist, it will be created
    - If a relative path is provided, it will be relative to the path of the previous WORKDIR instruction
    - Can use environment variables previously set using ENV in the Dockerfile
    - Example:
    -       WORKDIR /a
            WORKDIR b
            WORKDIR c
            RUN pwd
            # output: /a/b/c

            ENV DIRPATH /path
            WORKDIR $DIRPATH/$DIRNAME
            RUN pwd
            # output: /path/$DIRNAME

## Dockerfile Examples
[More Examples](https://docs.docker.com/engine/examples/)
### [Example](https://www.packtpub.com/books/content/understanding-docker)
-       
        FROM ubuntu:latest    
        MAINTAINER Scott P. Gallagher <email@somewhere.com>
        
        #Instead of starting and finishing process one by one, the **&&** helps to run one process to encompass the entire line  
        RUN apt-get update && apt-get install -y apache2
        
        #Add configuration file to the specified path and change the owner to the root user  
        ADD 000-default.conf /etc/apache2/sites-available/  
        RUN chown root:root /etc/apache2/sites-available/000-default.conf
        
        EXPOSE 80  
        #Run when the container is launched  
        CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]

### [Example](https://www.javacodegeeks.com/2015/08/getting-started-with-docker-from-a-developer-point-of-view-how-to-build-an-environment-you-can-trust.html)
- PHP
-       
        01  # Based on an example found at https://github.com/CentOS/CentOS-Dockerfiles  
        02  FROM centos:centos6  
        03  MAINTAINER Federico Tomassetti  
        04  RUN yum -y update; yum clean all  
        05  RUN yum -y install httpd; yum clean all  
        06  RUN yum -y install php; yum clean all  
        07  RUN yum -y install php-mbstring; yum clean all  
        08  RUN yum -y install php-mysql; yum clean all  
        09  RUN echo "Apache HTTPD" >> /var/www/html/index.html  
        10  EXPOSE 80  
        11  CMD exec /usr/sbin/apachectl -D FOREGROUND  

- MySQL
-       
        01  # Dockerfile  
        02  FROM centos:centos6  
        03  MAINTAINER Federico Tomassetti  
        04  RUN yum -y update; yum clean all  
        05  RUN yum -y install mysql-server; yum clean all  
        06  EXPOSE 3306  
        07  
        08  # Run a script to create a DB  
        09  ADD ./config_db.sh /config_db.sh  
        10  RUN chmod +x /config_db.sh  
        11  RUN /etc/init.d/mysqld start && /config_db.sh && /etc/init.d/mysqld stop  
        12  
        13  # Start Mysql and open a mysql shell just to keep the process alive:  
        14  # this is a poor-man trick and you probably want to do something smarter  
        15  CMD /etc/init.d/mysqld start && mysql  

### [Example](https://docs.docker.com/engine/reference/builder/#/dockerfile-examples)
-       # Multiple images example
        #
        # VERSION               0.1

        FROM ubuntu
        RUN echo foo > bar
        # Will output something like ===> 907ad6c2736f

        FROM ubuntu
        RUN echo moo > oink
        # Will output something like ===> 695d7793cbe4

        # You᾿ll now have two images, 907ad6c2736f with /bar, and 695d7793cbe4 with /oink.
