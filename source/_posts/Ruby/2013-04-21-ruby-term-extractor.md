title: Ruby Term Extractor
date: 2013-04-21 02:14:19
tags: [ruby]
---
# [Source Code](https://github.com/evenchange4/101-1_IR_PA1_extractor)
The Programming Assignment 1 report from [NTU101-1 IR Course](http://goo.gl/azVrC "Course")

## Getting Started
### Installing Ruby
If you do not have Ruby setup, please [install ruby](http://www.ruby-lang.org/zh_TW/downloads/ "ruby").

### Using extractor from the command line

``` 
$ cd code && ruby pa1.rb
```

<!-- more -->

## Architecture
### code/load/Extractor.rb
Extractor class

```
#IR PA1 MichaelHsu
#2012.09.18

#class : Extractor
#method: extract input string, output a text file

#https://github.com/raypereda/stemmify
require 'stemmify'

class Extractor
  def extract(collectionDocument, outputFileName)
		#use download stop_words @ http://ir.dcs.gla.ac.uk/resources/linguistic_utils/stop_words
		stopwords = open('load/stop_words.txt', "rb").read
		#output result array
		result = Array.new
		#Lowercasing everything.
		b = collectionDocument.downcase
		#Tokenization. char in a array []
		puts c = b.tr(',', '') .split(/[?."\\\s]/)
		#Stemming using Porter’s algorithm
		c.each do |term|
			#Stopword removal.
			if !stopwords.include?term
				result << term.stem
			end
		end

		#Save the result as a txt file.
		f = File.open("../output/"+outputFileName+".txt", 'w')
		f.write(result)
		f.close()

	end
end
```

### code/load/stop_words.txt
download [stop_words](http://ir.dcs.gla.ac.uk/resources/linguistic_utils/stop_words "download stop_words")

### code/pa1.rb
main code	

```
load 'load/Extractor.rb'

#open Text collection
a= open('../input/1.txt', "rb").read

#new object Extractor
extractor = Extractor.new  

extractor.extract(a, 'result')
```

### input
one [English news documents input file](https://ceiba.ntu.edu.tw/course/b079e8/content/1.txt "download")

```
the white house is also keeping a close watch on yugoslavia, where 
opposition forces are about to step up the pressure on president slobodan 
milosevic. but will it work? nbc's jim maceda is in belgrade tonight. 
serbia on the eve of a general strike. this two-hour roadblock is 
just a taste of what will come tomorrow, says the opposition, when 
a nationwide work stoppage is to begin. the purpose, to force out 
the man they say stole the presidential election, slobodan milosevic. 
the people don't accept to be victim and to be hostage of one person, 
milosevic. time is working against him now. but will the pressure 
work? four years ago, hundreds of thousands of serbs marched against 
milosevic calling for a general strike as well. the protest lasted 
three months, but milosevic survived. that's the real question. does 
the opposition have the power to confront the regime with its combined 
wealth to get rid of it? a case in point. these coal mine, the very 
heart of serbia's economy, now on strike, only a handful of workers 
guarding equipment. but with a week of electricity reserves, these 
engineers are worried about blackouts. "we're just making a gesture 
to try to make this regime come to its senses." there are difficulties, 
too, spreading the word about the strike. independent radio station 
b-292 has been shut down three times. its signal jammed in belgrade 
leaving journalists frustrated. i'm waiting for a better time when 
people in belgrade can hear and listening my radio. poorly organized 
opposition rallies have been shrinking, now drawing barely 10,000 
in belgrade. but opposition leaders hope all that will change tomorrow 
and civil disobedience throughout the land may give new life to their 
struggle to oust milosevic. jim maceda, nbc news, belgrade.
```

### output
An array result output output/result.txt 
example:

```
["white", "hous", "keep", "close", "watch", "yugoslavia", "opposit", "forc", "step", "pressur", "presid", "slobodan", "milosev", "work", "nbc'", "jim", "maceda", "belgrad", "tonight", "serbia", "gener", "strike", "two-hour", "roadblock", "just", "tast", "tomorrow", "sai", "opposit", "nationwid", "work", "stoppag", "begin", "purpos", "forc", "sai", "stole", "presidenti", "elect", "slobodan", "milosev", "peopl", "don't", "accept", "victim", "hostag", "person", "milosev", "work", "pressur", "work", "year", "ago", "hundr", "thousand", "serb", "march", "milosev", "call", "gener", "strike", "protest", "last", "month", "milosev", "surviv", "that'", "real", "question", "doe", "opposit", "power", "confront", "regim", "combin", "wealth", "rid", "case", "point", "coal", "heart", "serbia'", "economi", "strike", "hand", "worker", "guard", "equip", "week", "electr", "reserv", "engin", "worri", "blackout", "we'r", "just", "make", "gestur", "try", "make", "regim", "sens", "difficulti", "spread", "word", "strike", "independ", "radio", "station", "b-292", "shut", "signal", "jam", "belgrad", "leav", "journalist", "frustrat", "i'm", "wait", "better", "peopl", "belgrad", "hear", "listen", "radio", "poorli", "organ", "opposit", "ralli", "shrink", "draw", "bare", "10,000", "belgrad", "opposit", "leader", "hope", "chang", "tomorrow", "civil", "disobedi", "land", "new", "life", "struggl", "oust", "milosev", "jim", "maceda", "nbc", "new", "belgrad"]
```

### PA1.pdf
PA1 Spec from class

```
### Programming Assignment 1
Write a program to extract terms from a document.
#### You have to do:
	+ Tokenization.
	+ Lowercasing everything.
	+ Stopword removal.
	+ Stemming using Porter’s algorithm.
	+ Save the result as a txt file.
#### Text collection:
One English news documents
(https://ceiba.ntu.edu.tw/course/b079e8/content/1.txt).
### Please zip and submit 1.the result of document 1, 2.the
source code, and 3.a report to TA.
	+ 2 weeks to complete, that is 2012/10/2.
```

## Other Resources
1.  [Ruby String Classes](http://www.ruby-doc.org/core-1.9.2/String.html)
2.  [Stemming using Porter’s algorithm](https://github.com/raypereda/stemmify)
3.  [stop_words](http://ir.dcs.gla.ac.uk/resources/linguistic_utils/stop_words "download stop_words")