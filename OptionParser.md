## 示例
```

from optparse import OptionParser 
parser = OptionParser() 
parser.add_option("-p", "--pdbk", action="store_true", 
                  dest="pdcl", 
                  default=False, 
                  help="write pdbk data to oracle db") 
parser.add_option("-z", "--zdbk", action="store_true", 
                  dest="zdcl", 
                  default=False, 
                  help="write zdbk data to oracle db") 

(options, args) = parser.parse_args() 

if options.pdcl==True: 
    print 'pdcl is true' 
if options.zdcl==True: 
    print 'zdcl is true' 
```

## action

``action``包括 ``store``, ``store_true``, ``store_false``等

当``action``为``store``时，可以指定``type``, 如下所示:

```
parser.add_option("-f", "--file",
	              action="store",
	              type="string",
	              dest="filepath",
	              help="path of input file, the file should contain questions only")
```
