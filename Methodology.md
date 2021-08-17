# Some Short Methodology

## JavaScript Files
   - One
     1. Find JavaScript files 
     2. ffuf -w js_files.txt -u FUZZ -mr "sourceMappingURL"
     3. Download sourcemap
     4. https://github.com/chbrown/unmap
     5. Browse configs or just grep for API keys/Creds
   - Two
     1. ./dirsearch.py -u target -e php,html,js,xml -x 500,403
      2. found http://url.com/.svn/
      3.  clone & use https://github.com/anantshri/svn-extractor
      4. ./svn-extractor.py --url http://url.com --match database.php
      5. result in output dir and just open it
