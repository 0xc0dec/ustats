**ustats nginx module** provides basic statistics for nginx upstream servers:
* Number of requests
* Http 499/500/503 errors count
* Tcp errors
* Http read/write timeouts
* Fail timeout
* Max fails count
* Last failed access time
* Total fails count
* Blacklisted backend highlighting
* Down backends highlighting

The module's **web interface** provides a good visual representation of what's going on with your backends. Values in some columns of the table can be sorted within each upstream row.

Gathered data can also be retrieved in **JSON format**. To do so, append "?json" to the end of location on which the module was set to work on (see configuration instructions below).

![Screenshot](https://github.com/0xc0dec/ustats/wiki/shot1.png)

# Installation
_(tested with nginx 1.2.2)_

1. Copy ustats folder into your nginx/src/http/modules folder
2. Copy nginx.patch file into nginx root folder
3. cd into nginx root folder and apply the patch:
`patch -p1 -i nginx.patch`
4. Run `./configure` with all parameters you normally use, appending 
`--add-module=src/http/modules/ustats`
5. `make && make install`

# Configuration
## Example
    location /ustats {
        ustats memsize=3m;
        ustats_refresh_interval 6000;
        ustats_html_table_width 95;
        ustats_html_table_height 95;
    }

The module supports a number of configuration directives:
* syntax: **ustats memsize=\<size\>;**

  context: location

  Enables module handler for this location and sets the size of the shared memory that will be used to store
  statistics data across all worker processes. Example: `ustats memsize=2m;`


* syntax: **ustats_html_table_width \<number\>;**
  
  context: location

  default: 70

  Specifies web interface table width. Values less or equal to 100 are interpreted as percents, otherwise as
  pixels.


* syntax: **ustats_html_table_height \<number\>;**

  context: location

  default: 70

  See _ustats_html_table_width_.


* syntax: **ustats_refresh_interval \<number\>;**

  context: location

  default: 5000

  Specifies page refresh interval in milliseconds.
