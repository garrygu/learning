## Official Apache NiFi Documentation and Guides
- [Apache Nifi](https://nifi.apache.org/)
- [Overview](https://nifi.apache.org/docs.html)
- [User Guide](https://nifi.apache.org/docs/nifi-docs/html/user-guide.html)
- [Expression Language](https://nifi.apache.org/docs/nifi-docs/html/expression-language-guide.html)
- [Development Quickstart](https://nifi.apache.org/quickstart.html)
- [Developer’s Guide](https://nifi.apache.org/developer-guide.html)
- [System Administrator](https://nifi.apache.org/docs/nifi-docs/html/administration-guide.html)


## Resources
- [Apache NiFi Blogs](https://blogs.apache.org/nifi/)

- [Apache NiFi Mailing Lists](http://nifi.apache.org/mailing_lists.html)

- [nifi.rocks](http://www.nifi.rocks)

- [Apache NiFi Blogs by Datamelt.com](http://datamelt.weebly.com/blog/category/apache-nifi)


## Articles & Tutorials
- [Apache NiFi: Thinking Differently About DataFlow](https://blogs.apache.org/nifi/entry/basic_dataflow_design)

- [Apache NiFi, Not From Scratch](https://dzone.com/articles/apache-nifi-not-from-scratch)
> An introduction to implementing Apache NiFi, a flow-based data processing that analyzes your data in motion.

- [LEARNING THE ROPES OF APACHE NIFI](http://hortonworks.com/hadoop-tutorial/learning-ropes-apache-nifi/)

- [Apache Nifi (aka HDF) data flow across data center](http://blog.bikashagrawal.com.np/2016/01/29/apache-nifi-aka-hdf-data-flow-across-data-center/)
> Provides a step by step overview of how to setup cross data center data flow using Apache Nifi.

- [Using Apache Nifi to Stream Live Twitter Feeds to Hadoop](https://nedsblog.com/2015/09/02/using-apache-nifi-to-stream-live-twitter-feeds-to-hadoop/)
> To build an easy twitter stream data flow that will collect tweets that mentions the word “hadoop” over time and push these tweets into json file in HDFS.

- [Apache NiFi - Part 1 (Introduction)](https://www.linkedin.com/pulse/apache-nifi-part-1-introduction-neeraj-sabharwal)
- [Apache NiFi - Part 2 (Twitter Flow)](https://www.linkedin.com/pulse/apache-nifi-part-2-twitter-flow-neeraj-sabharwal)
>To display Tweets related to particular search terms.

**Slides**
- [Building Data Pipelines for Solr with Apache NiFi](http://www.slideshare.net/BryanBende/building-data-pipelines-for-solr-with-apache-nifi)

**Videos**
- [NiFi Tutorials (YouTube)](https://www.youtube.com/playlist?list=PLHre9pIBAgc4e-tiq9OIXkWJX8bVXuqlG)

- [Bulk Data Drops](https://www.safaribooksonline.com/library/view/analytic-data-storage/9781771375214/part24.html)


## Apache Nifi Dockerfile
- https://github.com/mkobit/docker-nifi  
Unofficial Apache NiFi Docker images

- https://github.com/apiri/dockerfile-apache-nifi  
Provides a Dockerfile and associated scripts for configuring an instance of Apache NiFi to run with certificate authentication.

- https://github.com/jdye64/docker-nifi  
Apache NiFi Docker Environment

- https://github.com/aperepel/docker-nifi  
Apache NiFi and Hortonworks Data Flow (HDF) Docker Environment with Clustering http://hortonworks.com/hdf/


## Similar Products
**ETL (Extraction Tranformation Loading) Tools**
- [Pentaho Data Integration (Kettle)](http://www.pentaho.com/product/data-integration)
> Talend uses an user friendly and comprehensive IDE (similar to Pentaho Kettle's) to design the procedures. Procedures can be tested on the IDE and compiled in Java code. The Java generated code can be modified to achieve greater control and flexibility. (http://www.robertomarchetto.com/talend_studio_vs_kettle_pentao_pdi_comparison)

- [Talend Open Studio (TOS)](http://www.talend.com/products/data-integration)
> Default ETL tool for the Pentaho ecosystem. On its very intuitive graphical editor (Spoon) it is easy to build Data Integration procedures. The procedures can be run by Kettle runtime in different ways: Using the command line utility (Pan), a small server (Carte), a database repository (Kitchen) or directly from the IDE (Spoon). Procedures are saved in XML files and interpreted by a Java library which is required to run the ETL tasks. (http://www.robertomarchetto.com/talend_studio_vs_kettle_pentao_pdi_comparison)