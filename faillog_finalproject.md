# Fail log: _Shawville Equity_ project
## July 29 2017
 
## WEEK 3 (JULY 24-JULY 30)
+ still unable to access dhbox, I downloaded 83471\_1890-01-02.txt (Equity issue for the 2nd of January 1890) and played around with regex cleaning in regexr and Notepad++.
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

## WEEK 4 (JULY 31-AUGUST 6)
### Antconc on the Equity for 1891
+ Loaded the equity files for 1891
+ using regex search, looked for /b\*day (note: returns "day", "holiday", "today" as well as days of the week: I'm just playing around for now.)
 + Collocates -> Sort by Freq(L)
  + one of the most frequent collocates is "church" (66 with 39 instances - not bad considering the wonky OCR) - I would have expected this kind of result; church was a common gathering place.
  + ottawa is also a high collocate (74 with 37 instances) suggesting either frequent travel between the regions or an interest in federal politics or both.
+ looking at the concordence plot for `Monday|monday`, it seems like my hypothesis from last week (that days of the week are more frequently mentioned in the summer months, when weather is better and more events are taking place) may have some evidence to support it!
+ strongest collocate terms for `Monday|monday` is "on" and "every" - make sense, and suggest again that they often refer to events (tho not necessarily social events)
 + "visit" is also a high collocate (8th with a stat of 9.02) - worth looking into
 + "arranged" is 50th with a stat of 8.86
 + "suicide" is 56th with a stat of 8.57???????! sensationalism???!
+ "church" for search `Monday|monday` is 63rd collocate with stat of 5.86; "church" for search `Sunday|sunday` is 9th (and the first characteristic collocate, eg not "on," "the," "of," etc...) with a stat of 8.52!
 + suggests that even with crappy OCR, Antconc is picking up some patterns that reflect real life!
+ idea: compare one year of the equity to whole decade
 + idea x2: rigorously clean one year of equity, use that to create a "calendar," and compare that equity year to the entire decade - maybe 2 decades - of less cleaned articles for context (maybe do 1900 vs 1890-1910)

### More regex
+ googled around for common OCR errors. Found a [list of 50,000 common OCR errors and their corrections for use with from 1700 to 1899]() compiled by Ted Underwood. Not sure exactly how I'd run all the code at once, but I ctrl+F'd the 50,000 errors for those related to days of the week: there were only about 80, and most were not mistakes I have seen in my perusal of the _Equity_ so far. 
 + I shared the list in Slack. I'll come back to it when I need it - I can run the relevant replacements with sed if I feel they'll make a difference.
+ played with other regex to find days of the week; best so far seems to be `+[A-Za-z ](day).+`
 + returns more than I need, but if I only make a detailed analysis of one year of the equity (12 months x 4 issues per month = 48 issues, a manageable number)
  + will analyze the rest of the equity thru antconc, topic modelling, etc etc.
+ `[montuewdhfrsan|MONTUEWDHFRSAN\ ]{3,6}day\b`
+ thinking about gephi and graphs and network analysis... I shouldn't do network analysis of dates vs events, should I? target/source doesn't work out well - I think it would be [multi-nodal](), eg not particularly reliable for analysis purposes.

### Network Analysis
+ should I use a network? what would my nodes be? I try to determine the feasibility of this kind of analysis using Scott B. Weingart's [Demystifying Networks](http://scottbot.net/lets-talk-about-networks/).
 + he suggests using nodes of the same kind and imagining edges as relationships
  + potential nodes & relationships: (Equity issue) contains mentions of (day of week) with edge weight being # of mentions in an issue
   + why? could show how season/time of year is related to events?
   + are they all really connected? Probably not. A bar graph or line graph would probably be a better visualization.
   
## A CSV of dates and events: 1895
+ my goal: to create a .csv file containing: the equity issue mentioning a day of the week, the day of the week mentioned, the line in which the day of the week appears
 + then, I can use this information to go through the .pdfs for 1895 and see what event each instance of a date refers to (this is necessary because the bad ocr didn't necessarily capture the articles as they appeared, but read across lines). this will also allow me to evaluate how well my `.+[A-Za-z ](day).+` regex locates days of the week.
+ began by moving all the 1895 files into a directory of their own, just to make sed and greping them easier: /equity/1895/
+ marked all lines containing a day of the week with this command: `sed -r -i.bak 's/(.+[A-Za-z ]day.+)/~\1/g' *.txt`
+ grepped all those lines into one file with command `grep '~' *.txt > dates.txt`
 + it's possible that other lines contained tildes due to OCR - I can locate those later with a regex command to find lines not contianing "day": `[^(day)]`?
 + I could have also regexed for a new line tilde `\n~` but... whatever.
+ I remove all commas with command `sed -r -i.bak 's/,//g' dates.txt` to simplify making the CSV
+ the filename contains the date of the equity issue, so I focus on making that the first column in my CSV. I delete the beginning of the file name: `sed -r -i.bak s/83471_//g' dates.txt`.
 + I remove the content after the date (in ##-##-## format) too, but then I realize it's more efficient to turn the end of each file name into a comma, so I restore the file and try: `sed -r -i.bak 's/.txt/, /g' dates.txt`
 + yay for backups!
+ try making format: date, day mentioned, full text of line with this command: `sed -r -i.bak 's/(.+),(.+)([A-Za-z ]{1,6}day)(.+)/\1, \3, \2\3\4/g' dates.txt`
 + close, but fail: `[A-Za-z ]day)` only returns 1 letter+day, I need to add a /b for command: `sed -r -i.bak 's/(.+),(.+)([A-Za-z ]{1,6}day)(.+)/\1, \3, \2\3\4/g' dates.txt`
  + nope... I try with \b: `sed -r -i.bak 's/(.+),(.+)(/b.+[A-Za-z ]day)(.+)/\1, \3, \2\3\4/g' dates.txt`
   + SUCCESS!!!
+ I try a few times to grep for lines not containing "day": `grep ack -L "day" dates.txt`, `grep -L "day" dates.txt`, `grep "[^day]" dates.txt` but none work
+ scrolling in nano, I realize the first character after the first comma in all lines that didn't get properly sed'd in my last few commands is a :, so I grep for `, :`
 + I sed those lines blank with `sed -r -i.bak 's/.+, ://g' dates.txt
 + I grep all lines with data on them with `grep '[^[:blank:]]' dates.txt > dates\_cleaned.txt
+ I transform into CSV with this command: `cp dates_cleaned.txt dates_cleaned.csv`
 + I forgot to add an initial column, so I add that: Equity issue, day of week, description (from line)
+ piped out history to project\_csvdates1895.md.

### OPENREFINE, CHECKING, AND CLEANING 1895 DATES
+used openrefine 2.7 to cluster some dates poorly recognized by OCR (Tueaday -> Tuesday, Mo nday -> Monday, Satorday -> Saturday, etc)
 + exported as .csv (dates\_cleaned-csv.csv) and opened in Notepad++ to edit.
+ off the bat, decide to delete some lines (especially those re: holday, these days, etc). kept entries referring to events like births, deaths, etc for now; they may be useful. Some examples removed:
   +`1895-01-03,holiday,  the holidays with her parents. 4`
   +`1895-01-03,next day,  The next day was the time appointed for a general council of war but while the council of war was proceeding the Chinese began to realize that the Japanese had established their mountain batteries on the hills commanding the left centre of the Chinese position and decided to advance out of Port Arthur and dislodge them. Ihen began`
   +`1895-01-03,every day,  animal I aim to feed the cows what sugar time beets they will eat up clean. The brood bows get a few roots every day and seem to eat and relish them as well as grai`
   +`1895-01-03,ten day,  No person shall injure or obstruct any fishway or do anything to deter or hinder fish from entering and ascending or descending the same or injure or obstruct any authorised dam under a penalty for each offence of not less than $2 nor more than |20 and an imprisonment of not less than two nor more than ten days in de fault of payment over and above all`
   +`1895-01-03,fifteen day,  Mr. Flynn's bill to amend the law respecting woods and forests oil public lands has in view the object of shortening to fifteen days the time necessary in giving notice of sale in seizure of timber limits by the Crown after remaining for two months in the custody of the agent or`
   +`1895-01-03,few day,  most of their liberty and filling tho apera- Persia Manchuria and Korea and as mat- departed. He came back in a few days ^owed when Pr0?e were good a larger ^`
   +`1895-01-10,cold day,  The Hon. Louis H. Davies M. P  says that the Dominion elections will be held before the winter is out is sure it will bo a cold day anyway it will Bro. Davies and the blizzard will probably strike Queen's P. E. I. so the`
   +`1895-01-10,to day,  of the Couaeivative party today. They`
   +`1895-01-10,the day,"  now. as in the days of our Savior men a consideration of the career of him whose can see the mote in their neighbors' eyea life we may have under review while perhaps blind to the beam in their Can the word great be legitimately ap own. Public men live now more than plied to Sir John Thompson in any or all ever in the full light that ia cast around ""f the various parts which he so honorably them from a hundred sources which did fulfilled? Undoubtedly some will answer not exist in vast ages. They cannot hide no either through a fear of being thought themselves behind the throne of their wanting in judicial acumen or perhaps"`
   +`1895-01-10,every day,  open to the criticism of the people aa exist in any every day dress unless it ia`
   +`1895-01-10,the day,  change until the day of his death was i the wounds of the heart. More than this	DSDaPtlllOIlt`
   +`1895-01-17,every day,  lion warn very simple	and neither animal	ten to twelve hoar# every day and they	this kind and jackets and pantaloons of	more than 200 years old.`
   +`1895-01-17,third day,  off. It leaves no soar. In addition the elrength of the poison. It will have no system must be ola&nsed. This is e good effeolou them at when they are ready complexion beautifies One part sulphur t0 ive u eome of their blood a neck vein one part cream of Urter. one part rhubarb wifl be opened and some blood drawn off. nil powdered and well mixed. Dose for an This blood will be treated so as to separate adult one teaspoonful in a wine glass of tke c|ot from tke watery part or serum water upon rising in the morning. Take jhe latter * the autitoxine. every third day for one month.`
   +`895-01-17,pay day,  hundred wholesale fur dealers with their A Lambeth hotelkeeper has been fined New Year iie the national pay day. All goods spread out on the ground and you $20 for allowing gambling on hie premises`
   +`1895-01-17,three day,  two tsblenpoonfule of powdered augur and ##til they	should run smoothly. Louis	young people go ih and pay homage to	She ie forced to fast three days then for	to attractive garniture ; if in November`
   +`1895-01-17,those day,  ble. Pour over it one quart of boiling table.	and all the officers go In and get down on I ceived this sum m cash.	were none in those days.`
   +`1895-01-17,Birthday,  Astrology and Birthdays.`
   +`1895-01-17,the day,  effective device which any orchard 1st can apply this one cannot be surpassed. It is an old-fashioned remedy bat it is as good to-day as it was in the days of our`
   + Interesting: a lot of entries with "few day" or "holiday" talk about people visiting; this could be useful data as visits can be considered social events. Perhaps I could use a range or an inexact date to process these events instead of throwing them out entirely. Examples: `1895-01-03,few day,  Mr. F. S. Roy is spending a few days 
 1895-01-03,few day,  Mr. Alex. Stewart returned home a few days ago and intends remaining with us. **	x;
 1895-01-03,few day,  Miss Grace McKechnie spent a few days in Eiinside last week.
 1895-01-03,Saturday,  Model school spent Saturday in this vi-`
 + had to double some entries because they contain reference to more than one date
   + WTF, line 37? `1895-01-10,nY5ire day,  2 nY5ire day Wlth Iheir daughter and T. Martin (equal.) %` what should I do with this??? is it a person's name??????
  + the issues have a clear order: local events -> fiction? -> advertisements -> international news. I'll check this against the PDFs and then perhaps focus in on local events.
  + oh my god, there's so much murder/drowning/suicide/burning to death :O u ok pontiac county?
   + sensationalist journalism?????!
   + left with a file of 834 lines: projects-dates-cleaned.csv

### Antconcing the Equity, Redux: 1890-1900
+ Imported a corpus of uncleaned Shawville Equity OCR into Antconc, from 1890 to 1900 inclusive
+ searched for clusters/N-grams occuring with "sunday": #1 with 241 hits across all 10 years is "Sunday school" - only "Sunday morning" and "Sunday last" come close, with 150 and 152 hits, respectively
 + obvious tentative conclusion (a la [planting beans in Martha Ballard's diary](http://www.cameronblevins.org/posts/topic-modeling-martha-ballards-diary/)) is that mass and church was important to the community - as mass would have often taken place Sunday morning.
   + I wonder if some events are more prominent in the morning vs the evening?
   + another common hit is "Sunday with" -> checking Concordance shows that many of these instances refer to people visiting Shawville, eg "X spent Sunday with relatives here." Never realized how important visits were.
+ export the results of clusters as antconc_results_sunday_equity_1890-1900.
+ next week, I'll run one of these for each day of the week to see if I can find other patterns. For now, onto the bulk of my work: comparing 1895 to the rest of the corpus.
+ ran a keyword list comparison with 1895 as my target corpus and 1890-1890 as my reference corpus
   + taking keyness significance rates from [antconc's readme](http://www.laurenceanthony.net/software/antconc/releases/AntConc335/help.pdf):
     + holy moly, the OCR for 1895 must have read "his" as "hie" a LOT, or else men in Shawville were being VERY possessive c. 1895. (Keyness of 640!!!)
      + some dude wrote a two stories about a dude named Henry Wyatt, so the keyness for Wyatt is high.
      + "shop" has a pretty high keyness at 92.957.
      + "Manitoba" has a really high keyness - 94.4 - probably due to the Red River Resistance!
      +" "cigar" pretty high - increased advertizing?
+ saved results as "antconc_results_keywordlist-1895vs1890-1900.txt".
  
### Rethinking my research question
+ so I set out on this with a poorly-defined idea of learning about "social events" or "community events" - eg, an answer to the question, "what did people DO in a rural, not to mention linguistically and culturally distinct, place like Shawville in the 1890s?"
 + problem: too narrow a definition of "community events". I wanted a Shawville community calendar, I got a newspaper.
+ solution: widen my search: Not only what do people DO, but how do people EXIST & IDENTIFY in a rural, not to mention linguistically and culturally distinct, place like Shawville in the 1890s?
 + the local, regional, and (possibly?) international events reported on by the Equity suggest how the people of Shawville saw these events as a part of their own existence as a community
  + in this context, even births and deaths can be read as "social events", because they changed the makeup of the community. Reports on national issues could show the community's shifting interests and political engagement (but for sake of time, I may cut out national & international entries from my 1895 csv, because I don't think I'll be able to analyze this line of inquiry as deeply as it deserves)
  
### Future work
+ clean up the descriptions column of CSV by checking it against the Equity .pdfs
+ run topic modelling on the .csv
+ visualize trends within models that contain words related to social events & bonds
+ compare 1895 to rest of period from 1890-1990
 + note compare > contrast: the data from the wider period can help show trends, contextualize 1895.
