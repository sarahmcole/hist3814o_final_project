# Fail log: _Shawville Equity_ project
## July 29 2017
 
### WEEK 3 (JULY 24-JULY 30)
+ still unable to access dhbox, I downloaded 83471_1890-01-02.txt (Equity issue for the 2nd of January 1890) and played around with regex cleaning in regexr and Notepad++.
+ trying to isolate proper nouns (words starting with capital letter...) `(\b[A-Z].+[^\s])`??
 + `\b[*. ][A-Z][a-z|A-Z]*` returns all capitalized words not at the beginning of a sentence
+ could I find all instances of dates WITHIN ARTICLES and use those to examine what kinds of events happened?
 + first must remove all non-article material (eg headers containing dates of publication)
 + could I extract the words AROUND each date, then use that to find what kinds of events were run? extract each LINE with a DAY in it? (sunday, monday, tuesday...)
 + went thru Equity for 2 January 1890 using `[A-Za-z]*day` regex  (note: not perfect - returns results for "today" and "holiday", etc; because I'm just playing around manually right now I just skip past those results - if I were to sed or grep something, I'd need to be more specific) and put in experimental XML tags: <event>, <eventDate>, and <location> for each social gathering referred to by day of event
  + could I later use these XML tags to create a csv like so: event, date, location, date of mention in Equity???
   + OOH!
+ I refer to my final project github repo to check what my ideas actually were...
 + hyperlinking: could I look for repeated words/events & hyperlink them together?
  + what (larger historical argument)[] could this serve?
+ started adding `<div>`, `<dateline>` and `<p>` tags to 2 Jan 1890 issue (according to [TEI guidelines](http://www.tei-c.org/release/doc/tei-p5-doc/en/html/DS.html)). Realized that this is a lot of work, and I probably won't be data mining this version of the text as DHbox still isn't up, so I stop a few stories in.
 + it's almost impossible to `<div>` some of these (eg the OCR for 9 January 1890) properly because of the way the OCR read the text across columns.... great.
 + looking into downloading Cygwin so I can do some greping and seding on my Windows machine. Good idea or no...?
+ can find instances of dates when properly spelled with regex `\b.+(Mon|Tues|Wed|Thurs|Fri|Satur|Sun)day.+`
 + problem: won't find split words or badly OCR'd words.
 + problem: `.+day.+` returns all lines with "day," "today," "holiday"...
+ observation: the word "left" seems to appear a lot with "___day" words. Maybe there IS nothing to do in Shawville - everyone is always leaving...
+ observation: why are some issues OCR so different? the OCR for Jan 02 is very readable compared to Jan 09.
+ tried combining commands from exercise 1 (using a tilde to isolate relevant lines) with my sed commands designed to find days. not very useful - too much info gets cut off due to bad OCR. still, I saved a grep example of lines containing pattern `.+day\b.+` as 05-07-1891-days.txt to see.
+ uploaded files mentioned in this section to /hist3814o_finalproject/ repo.