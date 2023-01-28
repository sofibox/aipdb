aipdb --version
=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=

Info: AbuseIPDB (aipdb) is bash script to manage AbuseIPDB API

Version: 0.1-beta

Author: Arafat Ali | Email: arafat@sofibox.com | (C) 2019-2023

=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=~=
This bash script is used to check if an IP or domain is on a blacklist by utilizing the API from AbuseIPDB.com


Script configuration:
You can insert your API key in the script by editing the config file aipdb.conf. The script will also prompt you to enter your API key if you did not insert it in the config file.

You can limit the script check output by editing the variable ABUSEIPDB_OUTPUT_MAX_LIMIT from the config file aipdb.conf. Default value is 500.

Script usage:
aipdb <+ action> <* options>, where + is required and * is

Script documentation

List of available actions:

check

        This action is use to query the AbuseIPDB.com API. Required option is --target or --ip-address

        example:
            ./aipdb check --ip 1.2.3.4
            ./aipdb check --ip-address 1.2.3.4
            ./aipdb check --domain sofibox.com
            ./aipdb check --domain-name sofibox.com
            ./aipdb check --target 1.2.3.4

        Note: The output above did not include reports or comments from users. To include reports and comments, use the --json option.
report

        This action is use to report an IP address to the AbuseIPDB.com API. Required option is --ip-address and (--category or --categories)

        Other optional option: --comment

        example:
            ./aipdb report --ip-address 1.2.3.4 --category 18 --comment "This IP is used for spamming"


Other optional options:

-h, --help
    This option is use to display the help message for the script

-v, --verbose
    This option is use to enable verbose output. You can use this option multiple times to increase the verbosity level (eg: -vvv)

    eg:
        ./aipdb check --ip-address 1.2.3.4 --verbose or ./aipdb check --ip-address 1.2.3.4 -v

-s, --scripting
    This option is use to enable scripting mode. When this option is enabled, you can the script status code to determine if the script was successful or not.

    eg:
        ./aipdb check --ip-address 1.2.3.4 --scripting
        echo $?
        80

        Note: The return value 80 above refers to the abuse confidence score (ACS) of the IP address. If the script return 0, it means that the IP address is not blacklisted.

-j, --json
    This option is use to enable json output. When this option is enabled, the script will output the result in json format.

    eg:
        ./aipdb check --ip-address 1.2.3.4 --json

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