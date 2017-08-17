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
  + what larger historical argument could this serve?
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

### Antconc on the Equity for 1891
+ Loaded the equity files for 1891
+ using regex search, looked for /b*day (note: returns "day", "holiday", "today" as well as days of the week: I'm just playing around for now.)
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

## WEEK 4

### NETWORK ANALYSIS
+ should I use a network? what would my nodes be? I try to determine the feasibility of this kind of analysis using Scott B. Weingart's [Demystifying Networks](http://scottbot.net/lets-talk-about-networks/).
 + he suggests using nodes of the same kind and imagining edges as relationships
  + potential nodes & relationships: (Equity issue) contains mentions of (day of week) with edge weight being # of mentions in an issue
   + why? could show how season/time of year is related to events?
   + are they all really connected? Probably not. A bar graph or line graph would probably be a better visualization.
   
## A CSV OF DATES AND EVENTS: 1895
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
 + I grep all lines with data on them with `grep '[^[:blank:]]' dates.txt > dates_cleaned.txt
+ I transform into CSV with this command: `cp dates_cleaned.txt dates_cleaned.csv`
 + I forgot to add an initial column, so I add that: Equity issue, day of week, description (from line)
+ piped out history to project_csvdates1895.md.

### OPENREFINE, CHECKING, AND CLEANING 1895 DATES
+used openrefine 2.7 to cluster some dates poorly recognized by OCR (Tueaday -> Tuesday, Mo nday -> Monday, Satorday -> Saturday, etc)
 + exported as .csv (dates_cleaned-csv.csv) and opened in Notepad++ to edit.
+ off the bat, decide to delete some lines (especially those re: holday, these days, etc). kept entries referring to events like births, deaths, etc for now; they may be useful.
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
 + had to double up an entry because it contained both saturday and monday
 + WTF, line 37? `1895-01-10,nY5ire day,  2 nY5ire day Wlth Iheir daughter and T. Martin (equal.) %` what should I do with this??? is it a person's name??????
+ the issues have a clear order: local events -> fiction? -> advertisements -> international news. I'll check this against the PDFs and then perhaps focus in on local events.
+ cleaned thru june
+ oh my god, there's so much murder/drowning/suicide/burning to death :O u ok pontiac county?
 + sensationalist journalism?????!
+ left with a file of 834 lines: projects-dates-cleaned.csv

### Antconcing the Equity, Redux: 1890-1900
+ Imported a corpus of uncleaned Shawville Equity OCR into Antconc, from 1890 to 1900 inclusive
+ searched for clusters/N-grams occuring with "sunday": #1 with 241 hits across all 10 years is "Sunday school" - only "Sunday morning" and "Sunday last" come close, with 150 and 152 hits, respectively
 + obvious tentative conclusion (a la [planting beans in Martha Ballard's diary]()) is that mass and church was important to the community - as mass would have often taken place Sunday morning.
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
 
### Cleaning & completing CSV
+ in notepad++, ran replace to correct some OCR errors:
 + `/bhie/b` -> "his"
 + `/bthie/b -> "the," "this"
 + using word prediction list in Notepad++, replaced various misspellings of "Shawville"
+ compared .pdf to my csv
 + as I expected, the OCR missed quite a few lines it should have caught.
 + transcribed articles describing each event into csv, without commas to preserve format.
+ sorted events into categories based on sxn of Equity it appears in.
+ fuck: should I have searched for "last week," "next week," "this week"? shiiit.
+ I clean all of January in this way - it takes hours. Will this be useful? I save this new transcribed version as project_dates_transcribed.csv.
 + I cleaned it more carefully, checking against the pdfs a second time, including all visits and events, even if a day of the week is not mentioned in the article. I record these events, and any events that don't have a clear date stated (eg. "last week," "holidays," "a few days") as "n/a." I also sort into types of social event: birth, death, event, visit (someone comes to Pontiac), departure (someone leaves Pontiac), church.
   + saved as "equity_transcript_january_final.csv.

### TOPIC MODELLING JANUARY 1895 SOCIAL EVENTS
+ imported equity_transcript_january_final.csv using RStudio's "Import Dataset" function
+ got my data into a mallet-friendly instance list using this tutorial: http://rci.rutgers.edu/~ag978/litdata/hw10/
+ imported into RStudio project january1895_topicmodel.Rproj and followed [Shawn Graham's Topic Modelling in R exercise](http://workbook.craftingdigitalhistory.ca/supporting%20materials/topicmodel-r-yourmachine/) to create files:
  +topic frequency across _Equity_ issues: january_1895_topics_docs.csv
  +topic label table: january1895_topiclabels.csv
  +word frequency table: january1895_wordfreqs
  +bar graph showing types of social occasions mentioned by each January 1895 Equity issue: occasionsbyissue_barplot_january1895.png
  +social occasions by approximate day they took place barplot: socialoccasionsday_barplot_january1895.png
  +type of social occasion by frequency: Types_barplot_january1895.png
  +wordclouds related to topics: wordcloud_public_january1895.png, wordplot_church_january1895.png, wordplot_diptheria_january1895.png, wordplot_evening_january1895.png, wordplot_meeting_january1895.png, wordplot_miss_january1895.png, wordplot_monday_january1895.png, wordplot_mrs_january1895.png, wordplot_ottawa_january1895.png.
  + uploaded these files and .Rproj file into github/hist3814o_finalproject/.

### TOPIC MODELLING 1895 SOCIAL EVENTS
+ imported projects_dates_cleaned.csv using RStudio's "import dataset" function.
+ fail: only about 388 of my 800+ rows of data show up. They also don't show up in excel; a chunk of dates went missing between March and June and possibly other sections. However, all 800+ lines are still there in notepad++, so I open the document in OpenRefine to check it out.
+ WELL... I FOUND [THE PROBLEM](/hist3814o_finalproject/image_csv_problem.png)
+ AHA! it was `""` stray quotation marks that were causing multiple lines to be read as one cell. I ran a replace in Notepad++ to get rid of all the quotation marks, and all 847 rows uploaded to OpenRefine fine.
+ saved as 1895_socialevents_cleaned.csv
+ imported into RStudio as 1895events_topicmodel.Rproj. Followed [Shawn Graham's topic modelling tutorial](http://workbook.craftingdigitalhistory.ca/supporting%20materials/topicmodel-r-yourmachine/) to create files:
   + graph showing distribution of social events by Equity issue: 1895_eventsbyissue_bargraph
   + topic frequency across issues: 1895_events_topicdocs.csv	
   + word frequency table: 1895_events_wordfreqs.csv	
   + topic labels table: 1895_events_topicslabels.csv
   + wordclouds based on topic model: wordcloud_1895events_church.png, wordcloud_1895events_day.png, wordcloud_1895events_days.png, wordcloud_1895events_present.png, wordcloud_1895events_sunday.png, wordcloud_1895events_thursday.png, wordcloud_1895events_widow.png, wordgraph_1895events_meeting.png.
   + uploaded these files and .Rproj file to github/hist3814o_finalproject/.
   
### NER
+ using [Stanford NER 3.8.0](https://nlp.stanford.edu/software/CRF-NER.shtml), I tagged equity_transcript_january_final.csv and 1895_events_cleaned.csv based on 3 values: <LOCATION>, <PERSON>, and <ORGANIZATION>
  + I save the results as equity_transcript_january_tagged.csv and 1895_events_tagged.csv.
+ following [Shawn Graham's regex + NER turotial](http://workbook.craftingdigitalhistory.ca/supporting%20materials/regex-ner/), I created a list of:
  + all locations mentioned in January 1895 in relation to social events: january1895_locations.csv -> see section "Locations in Palladio"
+ followed Dr. Graham's instructions using regex in Cygwin to find all <PERSON> tags... and fucked up completely.
  + saved results as failedregex.txt. Yay for backups.
  
### LOCATIONS IN PALLADIO
+ using Google Maps, I made a .csv with the locations mentioned in January 1895 related to social events and their coordinates on the map.
  + Initially I tried doing this automatically using [OpenRefine's named entity recognition](https://github.com/RubenVerborgh/Refine-NER-Extension), but it failed to recognize a lot of the smaller communities in Pontiac and misidentified communties that shared names with more famous communities (eg Bristol returned Bristol, England).
+ next steps: import this data into [Palladio](palladio.designhumanities.org) for visualization on a map.
+ I used Regex to isolate locations mentioned in January 1895, then manually geoparsed them (see section "Maps, geoparsing, and Palladio" for my more efficient geoparsing with Python.)
+ I loaded these into Palladio and played around with the data. Unfortunately I didn't remember to export it; this wasn't a huge loss, because I very quickly created better maps (see "Maps, geoparsing, and Palladio").
  
### NETWORKS IN GEPHI
+ I want to represent place & people relationships through a network. I want to eventually create a binodal network, showing how people relate to each other and how they relate to places in Pontiac county.
  + I start small, with the January transcriptions. If I have time I will continue up to 1895, and maybe (if I'm feeling wild), 1890-1900.
+ I open my equity_transcript_january_tagged.csv in excel and use some conditional formatting to help me find all cells with a <PERSON> tag. Then I manually create a source/target list of people who have made visits. If the target of a visit isn't clear, I put "friends" as a filler. If the person is returning home, I put "home". I will probably have to edit these out later, but it's easier to include them for completeness' sake right now.
  + Why am I not using a digital tool to create this list automatically? I'm tired and I wanted to do some mindless data crunching. Now onto Gephi, which is... not mindless.
+ I followed the [tutorial]() in the HIST3814O workbook to create a network showing the visits between people in January 1895 in Shawville. I run Modularity, which returns communities within the network, and color the network based on those communities.
  + saved my graph as graph_january1895_visits.pdf. Uploaded to github/hist3814o_finalproject/.
+ had to constantly reinstall Gephi because the preview window wasn't working. Seems like a common problem - a fix would be useful.
+ imported a .csv listing visits mentioned in January 1895: `place from, place to`. I ran Force Atlas 2 and calculated the PageRank (relative prestige) for each location, then sized the nodes based on PageRank.
   + Then, imported 2 .csvs dealing with people and places: `person, lives in`, and `person, visits`.
   + I ran Force Atlas 2, but I did not calculate PageRank. See [Scott Weingart's cautions on bimodal networks](http://www.scottbot.net/HIAL/index.html@p=41158.html). I still have the places sized by relative prestige; the nodes representing people simply help show how these people were connected by place.
   + In the future, I would like to differentiate between a `lives in` edge and a `visits` edge, but the graph is already complex enough and still useful for visualizing relationships between people and place, so I leave it for now.
   + I export as visitsjan1895network.graphml & visitsjan1895network.svg & visitsjan1895network.png. 

  
### TOPIC MODELLING 1890-1900 FULL TEXT
+ transformed my corpus into a .csv with the following format: `equity issue, text of issue in a single line`
  + I accomplished this through [experimentation, googling, and stackExchange](https://github.com/sarahmcole/hist3814o_final_project/blob/master/1890-1900complete_commands.md).
  + saved the resulting table as 1890-1900_complete_table.csv.
     + As with my 1895 CSV, I had to delete all `"` from my text in order for RStudio to import properly. 
+ Then imported into RStudio project 1890-1900_topicmodel.Rproj and followed [Shawn Graham's topic modelling tutorial](http://workbook.craftingdigitalhistory.ca/supporting%20materials/topicmodel-r-yourmachine/).
   + modelled 50 topics.
   + created the following files:
     + word frequency list: 1890-1900-word-freqs.csv
	 + topic labels: 1890-1900-topics-labels.csv
	 + frequency of topics across issues of the _Equity_: 1890-1900-topics-docs.csv.
	 + 12 wordclouds that seemed to deal with social events, social trends, or social visits: 1890-1900_wordcloud_annie.png, 1890-1900_wordcloud_april.png, 1890-1900_wordcloud_christmas.png, 1890-1900_wordcloud_feb.png, 1890-1900_wordcloud_mabel.png, 1890-1900_wordcloud_hester.png, 1890-1900_wordcloud_july.png, 1890-1900_wordcloud_lauraine.png, 1890-1900_wordcloud_nov.png, 1890-1900_wordcloud_ruth.png, 1890-1900_wordcloud_sept.png, 1890-1900_wordcloud_time.png.
	 + uploaded these files and R project files to github/hist3814o_finalproject/.
	 
### TOPIC MODELLING 1895
+ transformed my corpus into a .csv with the following format: `equity issue, text of issue in a single line`
  + saved the resulting table as 1895_complete_table.csv.
     + As with my 1890-1900 CSV, I had to delete all `"` from my text in order for RStudio to import properly. 
+ Then imported into RStudio project 1895_topicmodel.Rproj and followed [Shawn Graham's topic modelling tutorial](http://workbook.craftingdigitalhistory.ca/supporting%20materials/topicmodel-r-yourmachine/).
   + modelled 50 topics.
   + created the following files:
     + word frequency list: 1895complete-word-freqs.csv
	 + topic labels: 1895complete-topics-labels.csv
	 + frequency of topics across issues of the _Equity_: 1895completetopics-docs.csv.
	 + 12 wordclouds that seemed to deal with social events, social trends, or social visits: 1895complete_wordcloud_christmas.png, 1895complete_wordcloud_jan.png, 1895complete_wordcloud_july.png, 1895complete_wordcloud_june.png, 1895complete_wordcloud_march.png, 1895complete_wordcloud_ottawa.png, 1895complete_wordcloud_bride.png.
	 + uploaded these files and R project files to github/hist3814o_finalproject/.
	 
### ANALYSIS WITH VOYANT
+ created 2 corpuses in [Voyant]():
   + complete text of the _Equity_ for 1895
   + complete text of the _Equity_ for 1890-1900
+ FAIL: The supplemental issues of the _Equity_ often contained far less text and little content to do with social events, so they created noticable but meaningless dips in my visualizations. I deleted these documents from the corpus.
+ FAIL: using these two corpuses and the wordclouds generated from my topic modelling, I created 10 graphs showing frequencies of various topics over the entire corpuses of 1895 and 1890-1900. Unfortunately, I didn't realize that Voyant's .png export doesn't contain a legend!!!! which is stupid!!!
+ I recreated some of the more relevant graphs and exported them in web browser form:

### MAPS, GEOPARSING, AND PALLADIO
+ uploaded the same `place from, place to` .csv I used for network analysis to Palladio. Then, I also uploaded my geoparsed  list of locations mentioned in January 1895, january1895_locations.csv.
+ plotted the relationships between the places in Palladio, then added a map layer to make it look pretty. I screencapped the results and saved as palladiojan1895visits.png and palladiojan1895visitslarger.png.
  + I wanted to do something similar to my network analysis and include a list of people living or visiting each location, but I wasn't able to figure out how to link my `person, visits` and `person, lives` .csvs to the coordinates I uploaded in Palladio (as they were already linked to the `place from, place to` .csv, and Palladio doesn't seem to play well with linking more than two datasets).
  + Potential future fix: calculating the "weight" of each city as an integer, i.e. adding up # of people who live there + # of people who visited there (as noted in the _Equity_) and adding that as a third value to my coordinates list?
+ decided to see if I could create a map of all places mentioned in the _Equity_ for 1895, to compare the local visits for January to the overall geographic interests of the _Equity_.
  + used Stanford NER 3.8.0 and Regex in Notepad++ to isolate list of locations mentioned in _Equity_ of 1895: locations_list_1895.csv.
  + Followed [Fred Gibb's geoparsing with Python](http://fredgibbs.net/tutorials/extract-geocode-placenames-from-text-file.html) tutorial - at first I tried to do it on my Windows computer, but turns out it is VERY DIFFICULT to download Python packages (I needed [Request](http://docs.python-requests.org/en/master/user/install/)) on Windows...
     + I worked around this by installing the Request package and other required packages through Cygwin, then following Gibbs' turorial in Cygwin.
	 + resulted in a list of place names with geocodes that I then turned into a tab-separated value table using regex in Notepad++: geocoded-placelist.txt
+ loaded geocoded-placelist.txt into Palladio and saved screencaps of results as palladio1895world.png and palladio1895close.png. Also saved the data as PlacesMentionedEquity1895.palladio.1.2.9.json.

### QUICK TOPIC MODEL CHARTS WITH RAW
+ using Excel, transformed my data regarding topic model frequency across documents into the following format: `document number, value`. This allowed me to plot the frequency of one topic across time.
+ created scatterplots using RAW for the following datasets (file names refer to labels for each topic as generated by RStudio):
  + january_1895_topics_docs.csv: carnival.evening.friday.attended.held.jan1895.svg (carnival.evening.friday.attended.held.jan1895.json), evening.guests.time.year.singing.jan1895.svg (evening.guests.time.year.singing.jan1895.json).
  + 1895_events_topicdocs.csv: church.temperance.city.sold.society.1895events.svg (church.temperance.city.sold.society.1895events.json), sunday.school.service.prayer.church.1895events.svg (sunday.school.service.prayer.church.1895events.json).
  + 1895: baie.attending.hia.des.harvest.1895.csv (baie.attending.hia.des.harvest.1895.json), jan.carnival.discount.overcoats.dosen.1895.csv (jan.carnival.discount.overcoats.dosen.1895.json)
  + 1890-1900-topics-docs.csv: feb.wire.deacon.drama.current.1890-1900.svg  (feb.wire.deacon.drama.current.1890-1900.json), sept.harvest.sickle.kootenay.rig.svg (forgot to save .json).
### FUTURE WORK
+ finish paradata
+ regularize my file names, jfc
  + organize my github repo