IMPORTANT:
Bing API will be depreciated as of Aug 01, 2012. It means global search 
WILL NOT work any more in Crot. You can still use it for local 
detection. for local detection you do not need to set up AppID

README
--------

Crot Plagiarism plugin for Moodle 2.0

1. REQUIRED SOFTWARE AND SETTINGS

1.1 PACKAGES (apart form those ones that are required by Moodle): 
  php_soap is recommended for Moodle 2.0 and must for global search functionality in Crot
  php_zip is a must for Moodle 2.0 so we assume it is installed on your server
  
1.2 SETTINGS in php ini 
  allow_url_fopen = On

!!! as of August 1 2012 you may skip step 1.3 as Bing is not accessible anymore
1.3 Bing Application (AppID) key
You need to obtain the Bing Application ID key from the Microsoft to use the global search features.  
Open one of the following links to get the ID : 

http://msdn.microsoft.com/en-us/library/dd251020.aspx
or 
http://www.bing.com/developers

and follow the instructions to get the App ID.

INSTALLATION
~~0. If you are using MySQL or Maria DBMS you need to adjust some DB settings:
CHANGE THE VALUE OF NAME_MAX_LENGTH in moodle/lib/xmldb/xmldb_table.php  (line 47) TO 45~~ (Not needed anymore when using latest version. Take a look at the following commit: 105110e60d42ff372e74dd653b66f2f330f53c27.)
1. Untar the crot folder into plagiarism/crot. Make sure you rename module folder to crot!
2. Login to Moodle as an admin. Plugins check page will be opened. Click on Upgrade button at the bottom of the page. Moodle will setup the required tables for you.
3. Go to Advanced features in the admin's menu and check "Enable plagiarism plugins" option. Save changes. 
3.a. IMPORTANT: purge all caches from the admin interface
4. Open Plugins/Plagiarism prevention/Crot link from the admin's menu.
5. Check "Enable Crot". OBSOLETE because of Bing issue: Put Bing Application ID key in the appropriate text box. Save changes.
6. Other settings you may change later based on your experience with the plug-in. 
	The complete description of these settings is as follows:
		- Student disclosure: This text will appear on the assignment submission page when student work will be uploaded to the system.
		- Grammar size is size of text used to calculate one hash in document fingerprint. Recommended value is 30.
		- Window size represents how granular tech search will be. Recommended value is 60.
		- Colours are used for highlighting of similar text fragments in the side by side comparison of documents. But at the moment colouring function works only with the one colour (#FF0000).
        - Max Distance between hashes in the text cluster: This parameter is used for allocation of hashes into text clusters in the colouring of similar text fragments. Recommended value is 100.
		- Minimum cluster size is a minimum number of hashes in the text cluster used for colouring of similar text fragments. Recommended value is 2.
	    - Default Threshold: Assignments with similarity score less than threshold value are not displayed on Anti-Plagiarism - Assignments page.
   		- Global search threshold reserved for future development. Recommended value is 90.
   		- Global Search Query Size is the number of words in the query for global search. Recommended value is 7.
   		- Percentage of search queries for Web search is randomly selected percentage of queries from all search queries for Web search. Recommended value is 40.
   		- Number of web documents to be downloaded: How many web documents will be downloaded to your server from the list of possible sources on the web. Recommended value is 10.
   		- Culture info for global search: Culture information is used in queries for Bing search engine.
        - Maximum file size: Files with the size more than this value are not downloaded from the internet. Recommended value is 1000000.
7. OBSOLETE because of Bing issue: Select Test global search to run a quick test of global search. If it works you will see results of test search query on the next step.
8. We would really appreciate if you check Registration option and complete simple registration form. We are doing this continious survey to apply for funding to support project development. 
9. Click Save Changes button
10. IMPORTANT: You need to setup CRON running on your server to have Crot running. Please refer to the implementation section below. 

IMPLEMENTATION
1. Login as a teacher to Moodle.
2. Go to the course, then to the assignment that you want to be processed by the antiplagiarism plugin. At the moment the plugin works with the following types of assignments:
	- Advanced uploading of files
3. Click Assignment administration/Edit settings link.
4. In the Crot block set up search parameters.
5. Save settings and wait for the cron scheduler to process the assignments.
   IMPORTANT: setting cron on Moodle is covered in Moodle doc: http://docs.moodle.org/20/en/Cron
	EVEN MORE IMPORTANT: You need to set up cron running php command from the command prompt, NOT using wget command as it will cause cron timeout.
   If you are not sure how cron works on your server please ask your system administrator about it.
   Processing may take significant time: it depends on the server power and external channel bandwidth. Please be patient and wait for cronjob to be over.
   Global search consumes more time as it requires querying the search engine and downloading similar files from the Internet.
6. Go to the assignment, click "View submitted assignments" link and you will see the maximum similarity score for submitted files.
7. Open the link with similarity score and compare documents.

PLEASE Send your comments to (moodlecrot [at] gmail.com).
