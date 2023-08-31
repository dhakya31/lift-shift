# Steps to achieve CI CD in AWS
1. In AWS create security groups for
    1.ELB
      * HTTP - anywhere
      * HTTPS - anywhere
      * ssh - myip
    2. Tomcat
      * 8080 - ELBSG
      * ssh - myip
      * 8080 - myip
    3.Backend services
      * MYSQL/AUORA - TomcatSG
      * MEMCACHE 11211 - TomcatSG
      * rabbitmq 5672 - TomcatSG
      * alltraffic - BackendSG
      * ssh - myip
2. Create instances for 
    1. mysql CentOS Stream 9 (x86_64) instance which is  available in amis market place and attach the bash file which provided in userdata
    2. memcache CentOS Stream 9 (x86_64) instance which is  available in amis market place and attach the bash file which provided in userdata
    3. rabbitmq CentOS Stream 9 (x86_64) instance which is  available in amis market place and attach the bash file which provided in userdata
    4. tomcat instance ubuntu 22 
  
3. Now build the app locally