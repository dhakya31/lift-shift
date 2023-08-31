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
  
3. Now build the app locally using mvn install

4. Create use with s3 full access
  * aws configure
  * create s3 bucket and upload the war file from local to s3
      aws s3 mb s3://vpro-bucket
      aws s3 cp target/vprofile-v2.war s3://vpro-bucket
5. Create iam role with s3 full access and attach this role to tomcat instance and ssh instance and copy the war file from s3 bucket to tomcat instance
  * aws s3 cp s3://vpro-bucket/vprofile-v2.war /tmp/
  * stop tomcat service
    systemctl stop tomcat9
  * then remove
    rm -rf /var/lib/tomcat9/webapps/ROOT
  * and copy
  cp /tmp/vprofile-v2.war /var/lib/tomcat9/webapps/ROOT.war
  * and start tomcat service
    systemctl start tomcat9
      * for conformation 
      cd /var/lib/tomcat9/webapps/ROOT/WEB_NF/classes/appplication.properties