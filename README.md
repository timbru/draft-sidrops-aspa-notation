This repository contains the *intended* sidrops draft defining
a human readable ASPA notation.

We use the markdown document (.md) as the primary source, and
use 'mmark' to generate the XML document for uploading to the
IETF. 

The `mktext.sh` script can be used to run mmark locally. You
can verify that the resulting xml file is good by using the
IETF tools here: https://author-tools.ietf.org

GitHub will run xml2rfc using .github/workflows/xml2rfc.yml.
