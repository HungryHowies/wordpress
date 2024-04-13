# SSO Plugin and Cache

 The following documentaion will show what plugin/s to use and how to configure Cache.

Install Plugin
* miniOrange SSO using SAML 2.0
* (Optional) Sucuri (for extra securty)

Ensure the following is installed and configured.

## Cacheing

Edit file

```
vi /etc/php/8.1/apache2/php.ini
```

Adjust these lines.

```
opcache.enable=1
opcache.memory_consumption=128
opcache.max_accelerated_files=10000
opcache.revalidate_freq=200
```

Restart Apache2 service.

```
systemctl restart apache2
```

Install the following packages.

```
sudo apt-get install -y php8.1-fpm
```
```
sudo apt-get install -y imagemagick
```
```
sudo apt-get install -y php-imagick
```

Load imagick for PHP.

```
sudo phpenmod imagick
```

Restart these services.

```
systemctl restart php8.1-fpm
```
```
systemctl restart apache2
```

### miniOrange SSO using SAML 2.0

Following setup/configurations is for MiniOrange and Zitadel IDP.

Navigate to the Plugin MiniOrange --> tab called *Serice Provider Metadata*. 
Provide the following settings.

Example:
```
Identity Provider Name* : one20projects
IdP Entity ID or Issuer* : https://zitadel-build.domain.com/saml/v2/metadata
SAML Login URL* : https://zitadel-build.domain.com/saml/v2/SSO
```

**NOTE**: You will find this certificate using Zitadel end point *saml/v2/metadata*. It also can be downloaded after you make your Project/Application on Zitadel Web UI.

X.509 Certificate* :
```
-----BEGIN CERTIFICATE-----
MIIFITCCAwmgAwIBAgIBfjANBgkqhkiG9w0BAQsFADAsMRAwDgYDVQQKEwdaSVRB
REVMMRgwFgYDVQQDEw9aSVRBREVMIFNBTUwgQ0EwHhcNMjQwMjE5MjIwMzEzWhcN
MjUwMjE5MDQwMzEzWjAyMRAwDgYDVQQKEwdaSVRBREVMMR4wHAYDVQQDExVaSVRB
REVMIFNBTUwgcmVzcG9uc2UwggIiMA0GCSqGSIb3DQEBAQUAA4ICDwAwggIKAoIC
AQC2bD6RwBj8Rpxb9TnBqyKIjhTUhYEBBhOr3PoMnTp8a/KvMbcxBbTimvbY5rKY
............................................................Zmg
uRv9tKhCnnI1lddVRa/px46lkfkyFsiUNhFx8fdhPKiwWRgBXmUFTFwDOmYWPsdO
reB26xqhXLdNKyc0gvXHwp+WQpfTe45kuLHi4jq2DJ9h0hXe1/XZSv3gWzLumK1E
rouAm+dAdmeiUIZEheSY5L2MzEh/l0fUw1uHPT3GnicXbpB7Tjxa9Hwhk0uar6Ky
dLw36WQDeGTJjlca3G/6hJorR6ycJV3m0tSk6UeUrAgdy+rD9KxnENnAH2ISoOQf
5YBrjQkudCZsTReHJnkPjw1BiC8ozhjaNBR0TqYP8wOJwuYAukbsIVbZhHDZSBcx
jMYSVblwVlEZLMcsrLeeMcpRYRCDyaAABJ3tqawAUUkGAwX7+vuuVoOPBpGYYIzm
sA8fuzka0AFmNDKmeOT7La0hfP91
-----END CERTIFICATE-----
```
Navigate to the tab called "Service Provider Metadata".  Click the download button for the Metadata XML File save this file to be accessed later. WordPress Metedata XML file will be upload to Zitadel later when making Project/Application.

Next Configure Attribute/Role Mapping tab. Scroll down to "Role Mapping" and choose  the default role. In this example it will be set for Administrators.

![image](https://github.com/HungryHowies/wordpress/assets/22652276/fe5f1ad5-db4d-43dd-8fef-c0d940a27e79)

Once completed click the "Update" button.

**NOTE:** SSO User for WordPress has a Admin role when logging in.

# Zitadel Projects and Applications

Log into Zitadel instance.

Navigate to Organization ---> Projects.

Click "Create New Project" and set the project name, Click continue.

Under Application click "New".


There are two naming procedures.

1. Name the Project

2. Choose the SAML Application then name it (wordpress). Click continue.


Click the Application called Wordpress


![image](https://github.com/HungryHowies/wordpress/assets/22652276/e28d8975-77fe-4d24-aec8-1b44bd53dce1)


# Upload XML file.

The Metdata XML file that was downloaded from WordPress uploaded it to Zitadel

![image](https://github.com/HungryHowies/wordpress/assets/22652276/064bbdae-149b-40d2-8801-d29bf9c7df7c)


Click Save

Results:
```
<?xml version="1.0"?>
<md:EntityDescriptor xmlns:md="urn:oasis:names:tc:SAML:2.0:metadata" validUntil="2026-07-22T10:07:10Z" cacheDuration="PT1446808792S" entityID="https://howie.domain.com/wp-content/plugins/miniorange-saml-20-single-sign-on/">
    <md:SPSSODescriptor AuthnRequestsSigned="false" WantAssertionsSigned="true" protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
        <md:NameIDFormat>urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified</md:NameIDFormat>
	       <md:AssertionConsumerService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" Location="https://howie.domain.com/" index="1"/>
    </md:SPSSODescriptor>
	<md:Organization>
		   <md:OrganizationName xml:lang="en-US">miniOrange</md:OrganizationName>
	   	<md:OrganizationDisplayName xml:lang="en-US">miniOrange</md:OrganizationDisplayName>
   		<md:OrganizationURL xml:lang="en-US">https://miniorange.com</md:OrganizationURL>
	</md:Organization>
    	<md:ContactPerson contactType="technical">
   		<md:GivenName>miniOrange</md:GivenName>
	   	<md:EmailAddress>info@xecurify.com</md:EmailAddress>
	</md:ContactPerson>
   	<md:ContactPerson contactType="support">
	    	<md:GivenName>miniOrange</md:GivenName> 
		    <md:EmailAddress>info@xecurify.com</md:EmailAddress>
	   </md:ContactPerson>
</md:EntityDescriptor>
```


