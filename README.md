# Getting Started
Spring Cloud ecosystem demo bootstrap project

## Requirements
Java 15, Gradle 6.6+

Docker, docker-compose

### Host name aliases in hosts file
cat `127.0.0.1 discovery-service.com
127.0.0.1 discovery-service-1.com
127.0.0.1 discovery-service-2.com
127.0.0.1 discovery-service-3.com
127.0.0.1 discovery-service-4.com

127.0.0.1 gateway-service.com
127.0.0.1 gateway-service-1.com

127.0.0.1 configuration-service.com
127.0.0.1 configuration-service-1.com

127.0.0.1 client-service.com
127.0.0.1 client-service-1.com
127.0.0.1 client-service-2.com
127.0.0.1 client-service-3.com

127.0.0.1 tracing-service-1.com

127.0.0.1 db.com
127.0.0.1 db-1.com
127.0.0.1 db-2.com
127.0.0.1 db-3.com` >> /etc/hosts

## Prepare environment to run
`docker-compose up`

### Used project links
https://github.com/fdobrotv/springDiscoveryService
https://github.com/fdobrotv/springConfigurationService
https://github.com/fdobrotv/springCloudGateway
https://github.com/fdobrotv/springCloudClientService
https://github.com/fdobrotv/cloud_config