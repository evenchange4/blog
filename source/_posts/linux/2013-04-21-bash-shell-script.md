title: Bash shell Script
date: 2013-04-21 02:38:40
tags: [ruby]
---

## command line.md

```
# check bash path
$ which bash 
=> /bin/bash

$ chmod +x compile.sh 
$ ./compile.sh
$ chmod +x execute.sh 
```

```
$ ./execute.sh -r -i tmp2/queries/query-5.xml -o ans -m tmp2/model-files -d tmp2/CIRB010
```

<!-- more -->

## execute.sh

```
#!/bin/bash
FEEDBACK=false
while getopts ri:o:m:d: option
do
    case "${option}" in
  	        r) FEEDBACK=true;;
		i) INPUT=${OPTARG};;
		o) OUTPUT=${OPTARG};;
		m) MODEL=${OPTARG};;
		d) NTCIR=${OPTARG};;
    esac
done

if $FEEDBACK
then
	ruby main.rb -r-i $INPUT -o $OUTPUT -m $MODEL -d $NTCIR
else
	ruby main.rb -i $INPUT -o $OUTPUT -m $MODEL -d $NTCIR
fi
```

## main.rb

```
### read argument
require 'optparse'
$options = {}
OptionParser.new do |opts|
        opts.on("-r")                       { |s| $options[:r] = true }
	opts.on("-i", '-i INPUT', "input ") { |s| $options[:i] = s } # ./queries/origin/query-5.xml
	opts.on("-o", '-o OUTPUT',"output") { |s| $options[:o] = s } # ans
	opts.on("-m", '-m MODEL', "model ") { |s| $options[:m] = s } # ./model-files
	opts.on("-d", '-d NTCIR', "ntcir ") { |s| $options[:d] = s } # /tmp2/CIRB010
end.parse!
$dir_queryfile = File.dirname($options[:i]) 
puts $options
### end read argument
. . .
```