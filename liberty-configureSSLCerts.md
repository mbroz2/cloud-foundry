---

copyright: 
  years: 2020
lastupdated: "2020-04-09"
subcollection: cloud-foundry

---

{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Security Considerations

If you want to add your own certificates to the Liberty keystore, there are a few considerations to note. When a stand-alone application is deployed, a default Liberty configuration is provided for the application. You would need to configure an on-prem Liberty server and use one of the following methods for pushing your application: [server directory](/docs/cloud-foundry?topic=cloud-foundry-options_for_pushing#server_directory) or [packaged server](/docs/cloud-foundry?topic=cloud-foundry-options_for_pushing#packaged_server).

## Adding trusted certificates to Liberty
In the IBM Knowledge Center there are a few resources that will help guide you to create certificates in the Liberty keystore. 

- [Creating the Liberty keystore and certificate](https://www.ibm.com/support/knowledgecenter/en/SSXVXZ_2.1.8/com.ibm.i2.eia.go.live.doc/t_create_trust_i2a.html) : outlines the steps needed to use a self-signed certificate and creating a keystore that can then be used in a production environment.
- [Adding trusted certificates in Liberty](https://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/twlp_add_trust_cert.html) : this doc outlines the process of adding trusted certificates to Liberty truststore to secure communications with Liberty
- [Enabling SSL communication in Liberty](https://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/twlp_sec_ssl.html) : This section outlines the steps needed to add SSL configuration options including creating Liberty supported keystores i.e. Java Keystore (JKS) and setting SSL defaults in Liberty
- [SSL certificates for WAS Liberty Profile](https://www.ibm.com/support/knowledgecenter/en/SSZJPZ_11.7.0/com.ibm.swg.im.iis.found.admin.common.doc/topics/admin_mg_certs_waslib.html) : found under managing certificates section, this details changing the SSL server key for WAS Liberty profile and generating new keys.