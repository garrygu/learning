## Dockerfile examples

https://www.javacodegeeks.com/2015/08/getting-started-with-docker-from-a-developer-point-of-view-how-to-build-an-environment-you-can-trust.html
**PHP**  
>01  # Based on an example found at https://github.com/CentOS/CentOS-Dockerfiles  
>02  FROM centos:centos6  
>03  MAINTAINER Federico Tomassetti  
>04  RUN yum -y update; yum clean all  
>05  RUN yum -y install httpd; yum clean all  
>06  RUN yum -y install php; yum clean all  
>07  RUN yum -y install php-mbstring; yum clean all  
>08  RUN yum -y install php-mysql; yum clean all  
>09  RUN echo "Apache HTTPD" >> /var/www/html/index.html  
>10  EXPOSE 80  
>11  CMD exec /usr/sbin/apachectl -D FOREGROUND  

**MySQL**  
>01  # Dockerfile  
>02  FROM centos:centos6  
>03  MAINTAINER Federico Tomassetti  
>04  RUN yum -y update; yum clean all  
>05  RUN yum -y install mysql-server; yum clean all  
>06  EXPOSE 3306  
>07  
>08  # Run a script to create a DB  
>09  ADD ./config_db.sh /config_db.sh  
>10  RUN chmod +x /config_db.sh  
>11  RUN /etc/init.d/mysqld start && /config_db.sh && /etc/init.d/mysqld stop  
>12
>13  # Start Mysql and open a mysql shell just to keep the process alive:  
>14  # this is a poor-man trick and you probably want to do something smarter  
>15  CMD /etc/init.d/mysqld start && mysql  
