# Setting Up SSL Certification In ubuntu #
## Login to your server ##
### To generate a CSR run the command below in terminal: ###
1. openssl req -new -newkey rsa:2048 -nodes -keyout server.key -out server.csr
2. Two files will be generated on the same directry in which commands are run.
3. .csr file contains the CSR code that you need to submit during certificate activation. It can be opened with a text editor. Usually it looks like a block of code with a header: “-----BEGIN CERTIFICATE REQUEST----“ It is recommended to submit a CSR with the header and footer.
4. .key file is the Private Key, which will be used for decryption during SSL/TLS session establishment between a server and a client. It has such a header: “-----BEGIN RSA PRIVATE KEY-----“. Please make sure that the private key is saved as it will be impossible to install the certificate without it on the server afterwards.

##Get the Certificate generated for SSL Certificate Providers
1. https://www.namecheap.com
2. https://www.sslforfree.com/
3. https://mozilla.github.io/server-side-tls/ssl-config-generator/
4. https://www.ssllabs.com/ssltest/

##Installing a SSL certificate on Apache
1. Upload the files given by SSL Certificate Providers  with .key file generated in the beginning at "/etc/ssl"
2. cd /etc/apache2/sites-enabled/
3. cp "filename" "filename2" (Only One File exits there)
4. sudo nano "filename" 
5. Replace the code :
`code()`
<VirtualHost *:443>
	ServerAdmin admin@domainname.com
	DocumentRoot /var/www/html
	Servername domainname.com
	SSLEngine on
	SSLCertificateFile /etc/ssl/filename.crt
	SSLCertificatekeyFile /etc/ssl/filename.key
	SSLCertificateChainFile /etc/ssl/filename.ca-bundle
	<Directory /var/www/html>
		AllowOverride All
		Options FollowSymlinks
	</Directory>
</VirtualHost>

<VirtualHost *:80>
	Servername domainname.com
	Redirect permanent / https://domainname.com
</VirtualHost>

6. do replace domainname.com with your domain name and filename with the respective file name

## Reference
1. https://www.namecheap.com/support/knowledgebase/article.aspx/9446/0/apache-opensslmodsslnginx
2. https://www.namecheap.com/support/knowledgebase/article.aspx/9423/0/apache-opensslmodssl
