# NVD CVE Managment Tool v3.0
# Python v3.7
# Djang0 v2.2.2

The script and its documentation are taken from [cve_manager](https://github.com/aatlasis/cve_manager).
The improved script functions fine on python 3.7 and later, and some bugs are fixed that were associated with Python v2.7.

Python script for preparing NVD JSON feeds and importing in Local Database | Python 3.7 | Django 2.2.2

  a) parses NIST NVD CVEs, 
  
  b) prcoesses and exports them to CSV files, 
  c) creates a postgres database and imports all the data in it, and
  
  d) provides query capabilities for this CVEs database.

### Before Script execution create following directories.

`mkdir results`
`mkdir nvd`

### Create Virtual Env:

- `sudo apt install virtualenv`
- `virtualenv --python=python3 vepy3`
- `source vepy3/bin/activate`
- `python -m pip install -r requirements.txt`
- `python ./script.py COMMAND PARAMETERS AND SWITCHES`

Usage examples: 

- Download, parse and save in CSV files all CVEs from NIST NVD:

  `python ./script.py -d -p -csv`
  
- Create a postgresql database to host the downloaded CVEs:

  `python ./script.py -u <myuser> -ps <mypassword> -host <hostname or IP> -db <database_name> -ow <new_owner of database> -cd`

- Create the tables and views at the database:

  `python ./script.py -u <myuser> -ps <mypassword> -host <hostname or IP> -db <database_name> -ct`

- Import all data into the created database (requires the download, parse and sdtore as CSV files first, as explained above):

  `python ./script.py -u <myuser> -ps <mypassword> -host <hostname or IP> -db <database_name> -idb -p`

- Query for a specific CVE:

  `python ./script.py -u <myuser> -ps <mypassword> -host <hostname or IP> -db <database_name> -cve 2019-2434`
    
- Query for all CVEs related with a product (e.g. windows), with a base metric score greater than a value (e.g. 9, that is critcal), and a publication date equal or newer than a specific year (e.g. 2018):

  `python ./script.py -u <myuser> -ps <mypassword> -host <hostname or IP> -db <database_name> -pr internet -sc 9 -dt 2018`
  
- Query for all CVEs related with a vendor (e.g. microsoft), with a base metric score greater than a value (e.g. 9, that is critcal), and a publication date equal or newer than a specific year (e.g. 2019):

  `python ./script.py -u <myuser> -ps <mypassword> -host <hostname or IP> -db <database_name> -ve microsoft -sc 9 -dt 2019`
  
- Truncate the contents of all tables (required if you want to repeat the import process so as to update the data): 

  `python ./script.py -u <myuser> -ps <mypassword> -host <hostname or IP> -db <database_name> -tr`
  
- Delete the database (remove it completely):

  `python ./script.py -u <myuser> -ps <mypassword> -host <hostname or IP> -db <database_name> -dd`

Complete list of supported arguments:

  -h, --help            show this help message and exit
  
  -v, --version         show program's version number and exit
  
  -p, --parse           Process downloaded CVEs.
  
  -d, --download        Download CVEs.
  
  -csv, --cvs_files     Create CSVs files.
  
  -idb, --import_to_db  Import CVEs into a database.
  
  -i INPUT, --input INPUT
                        The directory where NVD json files will been downloaded, and the one from where they will be parsed
                        (default: nvd/)
                        
  -o RESULTS, --output RESULTS
                        The directory where the csv files will be stored (default: results/)
                        
  -u USER, --user USER  The user to connect to the database.
  
  -ow OWNER, --owner OWNER
                        The owner of the database (if different from the connected user).
                        
  -ps PASSWORD, --password PASSWORD
                        The password to connect to the database.
                        
  -host HOST, --host HOST
                        The host or IP of the database server.
                        
  -db DATABASE, --database DATABASE
                        The name of the database.
                        
  -cd, --create_database
                        Create the database
                        
  -dd, --drop_database  Drop the database
  
  -ct, --create_tables  Create the tables of the database
  
  -tr, --truncate_cves_tables
                        Truncate the CVEs-related tables
                        
  -cve CVE, --cvs_number CVE
                        Print info for a CVE (CVSS score and other)
                        
  -pr PRODUCT, --product PRODUCT
                        Print CVEs for a product
                        
  -ve VENDOR, --vendor VENDOR
                        Print CVEs for a vendor for all products
                        
  -sc SCORE, --score SCORE
                        Use base score as a selection criterion
                        
  -dt DATE, --date DATE
                        Use publication date as a selection criterion
