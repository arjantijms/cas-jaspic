# JASPIC(JSR-196) module for Jasig CAS #

This is the "Java Authentication Service Provider Interface for Containers"(JASPIC) module for Jasig CAS.
If you use this module, your application can be authenticated & authorized by the container with CAS.

## Supported Container ##
  * GlassFish V3

## Configuration(HTTP profile) ##
  1. Add a [CasLoginModule](https://wiki.jasig.org/display/CASC/JAAS+Integration) entry in your login configuration file(-Djava.security.auth.login.config).
```
cas {  
  org.jasig.cas.client.jaas.CasLoginModule required  
  ticketValidatorClass="org.jasig.cas.client.validation.Cas20ProxyTicketValidator"  
  casServerUrlPrefix="http://localhost/cas-server"  
  defaultRoles="";  
};
```
  1. Add the following jar files in your container's classpath.
    * cas-client-core.jar
    * commons-loggin.jar
    * cas-jaspic.jar
  1. Regist a this module as a ServerAuthModule in your container.
    * GlassFish
      1. Click `[Configuration]->[Security]->[Message Security]` and "New" button.
      1. Create a provider configuration as follows:
        * AUthentication Layer: HttpServlet
        * Provider ID: (unique name)
        * Default Provider: false (if true, all servlets will use this provider.)
        * Provider Type: server or client-server
        * Class Name: com.googlecode.cas.jaspic.servlet.CasServerAuthModule
        * Additional Properties
          * serverName: (your server name)
          * jaas-context: cas (set the entry name of CasLoginModule)
          * casServerUrlPrefix: (CAS Server URL)
          * casServerLoginUrl: (CAS Login URL)
          * defaultGroups: (set default group names)
          * groupAttributeNames: (set the attribute of CAS Assertion that has group names)
  1. Configure your application.
    * GlassFish (You can skip this step, if you set "Default Provider" true in the previous step.)
      * set the provider id at /sun-web-app/@httpservlet-security-provider in sun-web.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE sun-web-app PUBLIC "-//Sun Microsystems, Inc.//DTD Application Server 9.0 Servlet 2.5//EN"
"http://www.sun.com/software/appserver/dtds/sun-web-app_2_5-0.dtd">
<sun-web-app httpservlet-security-provider="CasServerProvider">
</sun-web-app>
```

## Configuration(SOAP profile) ##
> Just Making ...