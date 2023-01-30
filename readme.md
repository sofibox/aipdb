````
aipdb --version
=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=

Info: AbuseIPDB (aipdb) is bash script to manage AbuseIPDB API

Version: 0.1-beta

Author: Arafat Ali | Email: arafat@sofibox.com | (C) 2019-2023

=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=
````

This bash script is used to check if an IP or domain is on a blacklist by utilizing the API from AbuseIPDB.com

# Setup:

Download [maxibuild](https://github.com/sofibox/maxibuild) and run the following command to install it in your system (recommended for maintainability)

`maxibuild --install aipdb --force`

or 

You can also clone this repository and run aipdb directly from the repository folder.

```
git clone https://github.com/sofibox/aipdb.git
cd aipdb
chmod +x aipdb
./aipdb --version
````

# Script configuration:

````
You can insert your API key in the script by editing the config file aipdb.conf. The script will also prompt you to enter your API key if you did not insert it in the config file.

You can limit the script check output by editing the variable ABUSEIPDB_OUTPUT_MAX_LIMIT from the config file aipdb.conf. Default value is 500.
````

# Script usage:

````
aipdb <+ action> <* options>, where + is required and * is optional
````

# Script documentation

List of available actions:

`check`

````
        This action is use to query the AbuseIPDB.com API. Required option is --target or --ip-address

        example:
            ./aipdb check --ip 1.2.3.4
            ./aipdb check --ip-address 1.2.3.4
            ./aipdb check --domain sofibox.com
            ./aipdb check --domain-name sofibox.com
            ./aipdb check --target 1.2.3.4
            
            
            short version:
            ./aipdb check -t 1.2.3.4
            
            
        output:
            [aipdb->info]: [aipdb->scan]: Checking target IP address 1.2.3.4 ...
            AbuseIPDB scan results [new]:
            -------------
            IP address: 1.2.3.4
            Is public: true
            IP version: 4
            Is whitelisted: false
            Abuse confidence score: 44
            Country name: Australia
            Usage type: Data Center/Web Hosting/Transit
            ISP: APNIC Pty Ltd
            Domain: apnic.net
            Hostnames: []
            Total reports: 22
            Number of distinct users: 9
            Last reported at: 2023-01-28T05:31:31+00:00
            Last scan date: Sat Jan 28 13:41:31 +08 2023
            -------------
        
        Note: The output above did not include reports or comments from users. To include reports and comments, use the --json option.
````

`report`

````    
        This action is use to report an IP address to the AbuseIPDB.com API. Required option is --ip-address and (--category or --categories)
        
        Other optional option: --comment

        example:
            ./aipdb report --ip-address 1.2.3.4 --category 18 --comment "This IP is used for spamming"
            ./aipdb report --ip-address 1.2.3.4 --categories "1,2,5,18"
            
        output:
            
            [aipdb->info]: Reporting target IP address 1.2.3.4 ...

            AbuseIPDB report submitted result:
            -------------
            IP address: 1.2.3.4
            Abuse confidence score: 44
            Categories: 18
            Comment: This IP is used for spamming
        
        Note: For more information about the categories, please visit https://www.abuseipdb.com/categories

````

Other optional options:
````
-h, --help
    This option is use to display the help message for the script
    
-v, --verbose
    This option is use to enable verbose output. You can use this option multiple times to increase the verbosity level (eg: -vvv)
    
    eg:
        ./aipdb check --ip-address 1.2.3.4 --verbose or ./aipdb check --ip-address 1.2.3.4 -v
    
-s, --scripting
    This option is use to enable scripting mode. When this option is enable, you can get the script return value. This is useful when you want to use the script in another script.
   
    eg: 
        ./aipdb check --ip-address 1.2.3.4 --scripting
        echo $?
        80
        
        Note: The return value 80 above refers to the abuse confidence score (ACS) of the IP address. If the script return 0, it means that the IP address is not blacklisted.   

-j, --json
    This option is use to enable json output. When this option is enabled, the script will output the result in json format.
    
    eg:
        ./aipdb check --ip-address 1.2.3.4 --json
        
    output (sample):
    
        {
      "ipAddress": "1.2.3.4",
      "isPublic": true,
      "ipVersion": 4,
      "isWhitelisted": false,
      "abuseConfidenceScore": 44,
      "countryCode": "AU",
      "usageType": "Data Center/Web Hosting/Transit",
      "isp": "APNIC Pty Ltd",
      "domain": "apnic.net",
      "hostnames": [],
      "countryName": "Australia",
      "totalReports": 22,
      "numDistinctUsers": 9,
      "lastReportedAt": "2023-01-28T05:31:31+00:00",
      "reports": [
        {
          "reportedAt": "2023-01-28T05:31:31+00:00",
          "comment": "",
          "categories": [
            12
          ],
          "reporterId": 40115,
          "reporterCountryCode": "MY",
          "reporterCountryName": "Malaysia"
        },
        {
          "reportedAt": "2023-01-27T03:24:27+00:00",
          "comment": "Port Scan",
          "categories": [
            14,
            19,
            20,
            23
          ],
          "reporterId": 103305,
          "reporterCountryCode": "JP",
          "reporterCountryName": "Japan"
        },
        
        ... output was trimmed to save space
    
    
-c, --config
    This option is use to specify the config file to use. If this option is not specified, the script will use the default config file (aipdb.conf) in the same directory as the script.
    The script will create a new config file if it does not exist or it contains invalid data (you will be prompted to perform this action).
    
    eg:
        ./aipdb check --ip-address 1.2.3.4 --config /path/to/config/file
        
-o, --output
    This option is use to specify the output file to use. If this option is not specified, the script will use the default output file (aipdb_check_output.txt) in the same directory as the script.
    
    eg:
        ./aipdb check --ip-address 1.2.3.4 --output /path/to/output/file
        
-k, --cache
    This option is use to use previous scan output as cache. If this option is not specified, the script will perform a new scan.
    
    eg:
        ./aipdb check --ip-address 1.2.3.4 --cache

````
