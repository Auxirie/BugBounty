# Some Short Methodology

## JavaScript Files
   - One
     1. Find JavaScript files 
     2. ffuf -w js_files.txt -u FUZZ -mr "sourceMappingURL"
     3. Download sourcemap
     4. https://github.com/chbrown/unmap
     5. Browse configs or just grep for API keys/Creds
   - 
