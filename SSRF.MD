# SSRF

## SSRF Exploit
   - Internal port scanning
   - Leverage cloud services(like 169.254.169.254)
   - Use http://webhook.site to reveal IP Address & HTTP Library
   - Download a very large file (Layer 7 DoS)
   - Reflective SSRF? disclose local mgmt consoles
## Google cloud exploit
   - All in one payload to grab all internal metadata http://metadata.google.internal/computeMetadata/v1beta1/?recursive=true 
