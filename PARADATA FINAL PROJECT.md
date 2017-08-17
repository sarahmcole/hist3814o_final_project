# Paradata: What is there to do around here, anyway? Visualizing social ties in the Equity, 1895
## Sarah Cole
### HIST3814O: Digital History
### Professor Shawn Graham
### August 16 2017

# Goals of project
When I realized I would be working with a newspaper archive for the very rural Pontiac County, I couldn't help but ask: "what is there to _do_ in Shawville, anyway?" A jaded city-dweller, I wondered how I was going to spin _The Equity_ into a project I could get invested in. I picked a timeframe, the late Victorian era, on personal preference; but I still wondered what on earth people in Pontiac County would be doing that would be of historical interest. Then it dawned on me that my curiosity _was_ historical interest. How did these people spend their time? How did they relate to one another? What did Shawville do for fun?
As I became familiar with digital methodologies through Dr. Shawn Graham's digital history workbook and explored the _Equity_, these questions crystallized into more specific queries. I became interested in the way the _Equity_ seemed to serve as a channel for community announcements, almost like a modern-day social media network. The "Local and General" section of the paper contained updates on everything from dinners and departures to sermons and suicides. I saw that, while the _Equity_ contained announcements of local entertainment, the true scope of "community events" in the _Equity_ was much broader than I expected. I came to think of the _Equity_ as representing a "Victorian social network" - a paper-based, curated record of social events and the relationships they fostered.
The abilities of digital tools were also beyond my expectations. That being said, with limited time to work on my project, I had to content myself with outlining the potentials of these tools for analyzing data about Victorian social networks, rather than  Gradually, I focused on three goals for my project:
+ use digital tools such as topic modelling, corpus linguistics, and network analysis, and visualization to determine trends in the social events and relationships that appear in the Equity for 1895.
+ Compare these trends to the trends displayed in the Equity for 1890 to 1900, allowing me to determine if trends were stable over the late Victorian period, or if there were special considerations for certain years or a gradual change over time.
 + Ideally, I would like to go as far as 1930, encompassing the turbulent social period of WWI and the post-war economic boom. See section on "future work".
+ Using the data from 1895 and 1890-1900, contextualize the social events reported in the _Equity_ of January 1895. By combining distant and close reading, I hope to produce a detailed answer to my initial question, which I'm sure many people share: what was there to do in a rural Anglophone Quebec county in the 1890s? Presenting this in the form of a narrative helps illuminate the rich network of interconnected lives behind the _Equity_ data.
 + my initial goal was to produce such a calendar for all of 1895, but it soon became clear that the transcription process was too time-intensive for my timeline. See section on "future work".

# Collecting data

For this project, I data mined 573 issues of _The  Equity_, published from 1890 to 1900 in Pontiac county, QC, rendered into plain text files via Optical Character Recognition (OCR). This archive was established by Bibliotheque et archives nationale du Quebec, and can be accessed freely [here](http://collections.banq.qc.ca:8008/jrn03/equity/src/).

Initially, I stored the data on DHBox, a web-based workspace created for digital humanities students at Carleton University. After [the Great DHBox Crash of '17](https://electricarchaeology.ca/2017/07/25/uh-oh/), however, I contined the project on my own machine. As I am on a Windows computer, I used [Cygwin](https://www.cygwin.com/), an approximation of the Linux command line used by DHBox. I collected the text files using the open source tool [wget](https://www.gnu.org/software/wget/). 

The raw OCR data for the _Equity_ is very messy: it is full of typos, nonsensical strings of characters, and articles that run into one another. In the next section, I discuss the steps I took to clean this data. The archive also contains .pdf scans of the _Equity_, which were the basis for the OCR. These documents were extremely useful as I tried to determine the state of my text data.

I must also acknowledge the wide variety of secondary sources that guided me during this project. You can find most collected in [Shawn Graham's digital workbook for the course](workbook.craftingdigitalhistory.ca). I also supplemented the data gathered from the _Equity_ with data from Statistics Canada.

# Cleaning data

My first instinct was to clean up the poor _Equity_ OCR as accurately as possible. From my exercises in XML and transcription, I knew that a full transcription and XML markup of anything more than one or two issues of the _Equity_ was unfeasible. So, I started by isolating only the parts of the _Equity_ I was interested in, with the intetnion of transcribing just certain articles and sections of articles that contained information about social events.

To find social events, I used regex patterns in grep, sed, and Notepad++ to find lines of text within the OCR that contained references to days of the week. My final regex pattern to find these lines was `.+[A-Za-z ](day).+`. This returned most instances of the days of the week, even if they were considerably misspelled: as long as the phrase "day" was present, the line would be picked up (although most OCR tends to record "day" as "dav," I did not find this error to be particularly present in my corpus). It also returned instances of "today," "the other day," "everyday," "holiday," etc. 

I used more regex patterns to sort the lines into a .csv with the following format: `Equity issue, word containing "day" as picked up by regex, full text of line`. I imported this .csv into OpenRefine 2.7 and used the cluster command on the column containing days. This transformed most of the poorly OCR'd dates into recognizable words ("Tueaday" became "Tuesday," "Mo nday" became "Monday," etc).

As I cleaned the data, I realized that there were many entries picked up that did not contain data about social events. For example, this entry from January 3rd, 1895 clearly refers to political and military rather than local social events:
`1895-01-03,next day,  The next day was the time appointed for a general council of war but while the council of war was proceeding the Chinese began to realize that the Japanese had established their mountain batteries on the hills commanding the left centre of the Chinese position and decided to advance out of Port Arthur and dislodge them. Ihen began`
I began deleting entries like this one, paring down my document to 834 lines, corresponding to 834 "social events" in the Pontiac and surrounding regions. A lot of subjective decisionmaking went into this cleaning; this is probably one of the weakest points of my project. I did not clearly deliniate what I wanted to keep and what I wanted to delete before starting. I did not define "social events" before starting. Rather, my focus on local events, including visits, births, deaths, and community get-togethers, developed over the course of the cleaning process. At first, I copied every line I deleted to a separate document, so that I could go back and reinsert them if they became relevant. This took up a lot of time, however, so I stopped. I cannot escape the fact that I may have skewed my data considerably in this process.

Initially, my solution to this was to compare my list of events to the .pdf files of the _Equity_ and transcribe in any social events - which, by now, I had taken to mean passages in the _Equity_ that illustrated local community events and relationships between people within that community - that the OCR missed or that I overlooked in my cleaning. My goal was to create a full "social calendar" of 1895, which I could then use to narrate "a year in the life" of Pontiac county.

It quickly became clear that this was _not_ feasible within the timeframe I had for this project. After consulting with Dr. Graham, I decided to do a partial transcription and compare that to 1) my cleaned events list, and  2) to my uncleaned OCR data.

I completed transcriptions for January 1895, developing eight broad categories that I documented in a fourth column on my .csv: birth, death, arrival, departure, event, and church. 
  +"Birth" and "death" are self-explanatory;
  +"arrival" and "departure" covered announcements of peoples' comings-and-goings in Pontiac, which I divided into two categories in order to facilitate directional analysis
  +"event" covers miscellaneous community events, like fairs and meetings
  +"church" covers all events related to religion, of which there were enough to merit their own category. 
  
I also recorded the part of the paper in which the article appeared. This helped me later locate the events, because most of them appeared under sections that indicated which part of Pontiac the correspondant wrote from. These two categorizations wound up being potentially more useful than the clean OCR, for reasons I'll discuss in the next section.

As I went through the data, I realized that visits were an integral part of the social scene in Pontiac county in 1895 (this was partially a quirk of the dates I happened to look at; see the next section for more details). I decided to look at the spatial dimensions behind these social relations. I used the Stanford NER to tag people, places, and organizations in my transcribed list of January 1895 events. Using regex and Notepad++, I transformed my list of events into seveal different CSVs that would allow me to illustrate the movement of people through Pontiac county:
  + a .csv containing all the visits, arrivals, and departures mentioned in the Equity by the names of the people involved. I took a `source, target` approach to this data, to facilitate future analysis. `Mrs. Mulligan, Mr. Grant` means that Mrs. Mulligan visited Mr. Grant. As many of the visits did not contain exact dates (the typical article said something along the lines of "Mrs. Mulligan visited for a few days and departed last Sunday") I didn't try to track the time of visits.
  + a .csv containing all the people mentioned in the visits, arrivals, and departures .csv and where they lived. `Mr. Grant, Shawville` means that Mr. Grant lived in Shawville.
  + a .csv containing all the people mentioned in the visits, arrivals, and departures .csv and where they visited. `Mrs. Mulligan, Shawville` means that Mrs. Mulligan visited Shawville.
  + a .csv containing all the visits, arrivals, and departures metnioned in the _Equity_ by the places travelled to and from. Again, I used a `source, target` format: `Quyon, Shawville` meant that someone travelled from Quyon to Shawville.

Looking back, I could probably have automated the creation of these files with a Python program, or I could have created one master .csv with all this information and copied data from it as needed. That being said, with my limited timeframe separating out the .csvs allowed me to 1) get a deeper understanding of my data while still making progress manipulating it; and 2) quickly import the data into analysis tools that work best with simple datasets, such as Gephi and Palladio.

After I had created this detailed view of January 1895, I decided that I wanted to compare this data against a broader corpus. To facilitate topic modelling of the _Equity_, I created two .csvs that could be easily read by analysis tools:
  + one .csv containing all 54 issues of the _Equity_ for 1895. Each line contained the date of publication in YYYY-MM-DD format and the text of the issue in a single line.
  + one .csv containing all 584 issues of the _Equity_ for 1890-1900, inclusive (I chose to use 11 years rather than 10 just to get the round number). Each line contained the date of publication in YYYY-MM-DD format and the text of the issue in a single line.
The text of these corpuses was not modified in any way beyond the transformation into a .csv. This caused some problems in my analysis, but after the work I had already put into cleaning 1895 and January of 1895, I decided that I wanted to check if my effort made any significant difference in the ways I could analyze the _Equity_.

# Analyzing data

Once my data was cleaned, I essentially had four "levels" of data to work with:
  + the events of January 1895, transcribed, with extra dimensions such as type of event and section of paper
  + the events of 1895, extracted from OCR via regex
  + the entirety of OCR for 1895
  + the entirety of OCR for 1890-1900.
In order to compare the usefulness of all four levels of data, and make sure that any conclusions I drew were not the result of some quirk of my data cleaning, I had to do my best to use all for levels with each analysis tool I used. Were I to redo this project, I think I would limit my scope: perhaps only use an extracted list of events from 1895 and the entirety of OCR for 1890-1900.

My first attempts at analysis were using AntConc. I ran each of my datasets through AntConc and looked for patterns around the terms I had noticed while cleaning data. I found some promising figures suggesting that the OCR files, though they are full of typos - "hie" was one of the most common terms in the corpus, a misreading of "his." Despite these issues, "Sunday" was strongly correlated to "church," and "school" in the 1890-1900 corpus meaning that it picks up some of the patterns I expected to see in an analysis of social events in 1895.

I moved on to topic modelling, hoping to find some interesting themes related to social events throughout the corpus. I ran each level of data through the steps of [Shawn Graham's topic modelling in RStudio tutorial](http://workbook.craftingdigitalhistory.ca/supporting%20materials/topicmodel-r-yourmachine/) using R and RStudio 1.0.513. I ran 50 topics on each dataset, except for the transcribed January 1895 events list, on which I ran 25 because I felt that number was sufficient for the smaller amount of data. From each dataset I created three CSVs:
   + a .csv containing word frequencies across the corpus
   + a .csv containing topic labels for each topic, consisting of some of the most frequently-used words in the topic
   + a .csv containing the frequency of each topic across the corpus.

Each corpus returned a few topics related to social events and a few topics that seemed unrelated. In particular, the two uncleaned corpuses returned a couple of topics of header or giberish text; all four corpuses also returned topics including "Shawville" and "Thursday" in some variation, reflecting the publication date of the _Equity_ rather than any new information. I think topic modelling would work better on a spellchecked corpus with document headers removed, but otherwise unmanipulated.

I confirmed this suspicion when I tried graphing the frequency of topics across my corpuses using RAW. The topics derived from my two events lists were too specific and narrow to show any meaningful change over time, which is of course what I'm looking for as a historian. This occured because most of the topics emerged from the text of one or two events that occured in 1895, so when plotted on a scatterplot in RAW, the data showed one or two concentrations of data instead of a trend.

My graphs for January 1895 social events:

Topic: "carnival evening friday attended held"
Topic: "evening guests time year singing"

My graphs for 1895 social events:

Topic: "sunday school service prayer church"
Topic: "church temperance city sold society"

These topics contain words that are clearly related, but the graphs show that this is no great discovery, for the algorithm simply picked up one or two specfic events - like the carnival held in January 1895 - and generated topics with the surrounding words. These topics tell us about kinds of events that were held, but not much about how frequently they were held or whether their popularity changed over time.

The graphs developed for the full texts of 1895 and 1890-1900 were slightly more revealing. They still did not show major trends over time. I suspect that I would need a broader corpus, perhaps reaching into the 1920s or 1930s, to see meaningful trends in changing social patterns. A lot can change in a decade, but the social patterns of an entire community, especially a tight-knit rural community, don't shift that quickly. 

Still, these topic models helped me see how 1895 was or was not a typical year for Pontiac county; if the topics generated from my 1895 events data were similar to the topics generated over the entire corpus, I could assume that these social patterns were not just quirks in my data collection and cleaning.

My graphs for 1895 corpus:

Topic: "baie attending hia des harvest"
Topic "jan carnival overcoats dosen"

My graphs for the 1890-1900 corpus:

Topic: "feb wire deacon drama current"
Topic: "sept harvest sickle kootenay rig"

These graphs were interesting because they showed:
   + a high correlation between January 1895 and the carnival held in Ottawa that month, suggesting that an event of that size didn't happen very often 
   + a low correlation between 1895 and the relationship between "feb[ruary]" and "drama." It seems like whatever events caused this correlation, they began several years later, around the end of the decade. This is a great example of how digital tools can make research more efficient - I discovered this without close reading a single issue of the _Equity_ published in February. Thanks to topic modelling, if I wanted to know more about whatever "drama" was put on in Shawville in February, I know which years to focus in on.
   + "September" and "harvest" were always relatively closely related, in keeping with the expected seasonal rhythm of social celebrations in a farming community.
   
The most useful thing generated from these topic models, however, were the wordclouds. While wordclouds are not particularly useful for rigorous analysis, the following visualizations did help me see connections between certain words, and how the correlations between different words changed between my datasets.

Wordclouds for January 1895 events:

Wordclouds for 1895 events:

Wordclouds for 1895 full text:

Wordclouds for 1890-1900 full text:

These wordclouds show interesting, though not always statistically relevant, correlations between certain times of year and certain events. It seems singing was mentioned quite a bit in the winter months, and cabins moreso in the warmer months.

The prominence of names and words related to visiting in the wordclouds made me realize how important visits and personal relationships were to social life in Shawville during this decade. This discovery really directed my further analysis. I was particularly intregued by the grouping of visiting words like "depart," "guest," and "friend" with female names - "annie," "mabel," "miss," and "mrs" all occur more frequently in topics dealing with social networks than they do in other topics.

To further investigate this, I analyzed the full text of 1895 and 1890-1900 in [Voyant Tools](voyant-tools.org). This allowed me more control in visualizing trends in the data than topic modelling. For example, I could graph out the use of specific words over time. Drawing related words from my wordclouds, I created the following graphs to see how words within topics related over time.

Voyant word frequency graphs for [1890-1900 corpus:](http://voyant-tools.org/?corpus=7f30c8605fa0f24c1711af8d1b0699e7)

["mr" "friend*" "visit*"](http://voyant-tools.org/?corpus=7f30c8605fa0f24c1711af8d1b0699e7&query=friend*&query=visit*&query=mr&bins=66&view=Trends)
["mrs*" "friend*" "visit*"](http://voyant-tools.org/?corpus=7f30c8605fa0f24c1711af8d1b0699e7&query=mrs*&query=friend*&query=visit*&bins=66&view=Trends)
["miss*" "friend*" "visit"](http://voyant-tools.org/?corpus=7f30c8605fa0f24c1711af8d1b0699e7&query=friend*&query=miss*&query=visit*&bins=66&view=Trends)
["christmas" "rink" "musical"](http://voyant-tools.org/?corpus=7f30c8605fa0f24c1711af8d1b0699e7&query=christmas&query=rink&query=musical&bins=66&view=Trends)
["jan*" "carni*"](http://voyant-tools.org/?corpus=7f30c8605fa0f24c1711af8d1b0699e7&query=jan*&query=carni*&bins=66&view=Trends)
["sept*" "harve*" "fair"](http://voyant-tools.org/?corpus=7f30c8605fa0f24c1711af8d1b0699e7&query=sept*&query=harve*&query=fair&bins=66&view=Trends)
["feb*" "drama"](http://voyant-tools.org/?corpus=7f30c8605fa0f24c1711af8d1b0699e7&query=feb*&query=drama&bins=66&view=Trends)
["april*" "cabin"](http://voyant-tools.org/?corpus=7f30c8605fa0f24c1711af8d1b0699e7&query=april*&query=cabin&bins=66&view=Trends)

Voyant word frequency graphs for [1895 corpus:](http://voyant-tools.org/?corpus=bbdeaaf79478c871b4adf68246ed0f5c)

["miss" "guest*" "friend*" "visit*"](http://voyant-tools.org/?corpus=bbdeaaf79478c871b4adf68246ed0f5c&query=friend*&query=visit*&query=miss&query=guest*&bins=50&view=Trends)
["mr" "guest*" "friend*" "visit*"](http://voyant-tools.org/?corpus=bbdeaaf79478c871b4adf68246ed0f5c&query=guest*&query=visit*&query=friend*&query=mr&bins=50&view=Trends)
["mrs" "friend*" "visit*" "guest*"](http://voyant-tools.org/?corpus=bbdeaaf79478c871b4adf68246ed0f5c&query=mrs*&query=friend*&query=visit*&query=guest*&bins=50&view=Trends)
["ottawa" "carni*" "jan*"](http://voyant-tools.org/?corpus=bbdeaaf79478c871b4adf68246ed0f5c&query=jan*&query=carni*&query=ottawa&bins=50&view=Trends)

These graphs are generally more useful for understanding social events in relation to time than the topic models. For example, looking at the graph for "jan*" and "carni*" for 1890-1900, I can see that the carnival seems to have been a semi-yearly event in January. Both peak at the same points, suggesting that the carnival was only really mentioned in the month of January. It also shows that the carnival didn't really get started as an important event until 1895. Conversely, it seems like "cabin" didn't get strongly associated to "april" until later in the decade; perhaps the number of people vacationing in the region grew, or maybe there's a political reason (as the algorithms likely wouldn't distinguish between a cabin and the Cabinet) or a development in the region. Though these graphs don't show a clear trend in change over time, they can indicate where to look to discover the factors that influenced social live in the Pontiac over this decade.

The graphs tracking gendered titles ("mr," "mrs*," and "miss*") against words about visiting are fascinating. They don't correlate exactly, but a quick glance shows a much stronger link between women and social visits than between men and social visits. Men get mentioned in many more contexts than women do. This suggests to me that womens' participation in public life was limited in some significant way to the social sphere. Visiting, then, could be one of the few ways that women could participate in a (semi-)public life in Pontiac, which was otherwise predominantly agricultural - a farmer's society. This could also account for the prevalence of church-related events, at which women would have likely been welcome. I wasn't able to go much farther with this analysis, but I discuss the implications further in the "Future work" section.

Finally, the graphs tracking words about visiting revealed one quirk of my data cleaning. It seems like mentions of visits peak around January. A less significant peak occurs around Easter, which was on April 14th in 1895.This is not a pattern I would have noticed without digital tools, but it makes sense. After spending Christmas or Easter at home, people made the rounds of holiday visits (today, I suppose the modern equivalent would be families who jet off to the Carribean the week after Christmas). 

With this in mind, I wasn't surprised that so many of the visits seem to be personal affairs - visits to family, to close friends, with the occasional visit from a notable person (such as the local "school inspector"). The _Equity_ tracked many more social _connections_ than it did social _events_. Going into my project, I expected to find tons of mentions of all the pretty, idyllic, rustic celebrations seen in after-school-specials about rural communities: fairs, school plays, amateur concerts, and late nights spent on the skating rink (alas, the one party held on Shawville's skating rink in January 1895 turned out to be a "disappointment"). I now suspect that, in a county like Pontiac, people took their leisure time where and when they could - by visiting friends when they could, going to church, and taking advantage of the holidays to hold more concerts and parties than would normally be possible with the usual rhythms of working life. It seems like holidays, piety, and keeping up personal relationships justified taking a bit of time away from what was otherwise an existence centred on work and home. Hosting large social events wasn't a priority in and of itself.

Still, the fact that these small reprieves get so much mention in the _Equity_ suggests the important role that the paper played in encouraging local community. I noticed when looking through the January 1895 _Equity_ that the first page was dominated by advertisements and the "Local and General" section. World news, and even Canadian political news, was relegated to the back pages. To visualize the relative importance of the local and the global in the _Equity_, I decided to make some geographical visualizations.

I loaded two sets of geotagged data into [Palladio 1.2.9](http://hdlab.stanford.edu/palladio/) and produced two maps:
   + a map of [local visits in January 1895](https://github.com/sarahmcole/hist3814o_final_project/blob/master/Palladio-visits-jan1895.json), showing the movement of people from their place of residence to the destination of their visit. The size of each point represents the number of visits that went to or from that place.
   + a map of [all places mentioned in the full text of the _Equity_ for 1895](https://github.com/sarahmcole/hist3814o_final_project/blob/master/PlacesMentionedEquity1895.palladio.1.2.9.json), showing the geographical locations of the places mentioned by the paper in any context. The size of each point represents the number of times that place was mentioned by the paper.
The map of visits is interesting because it shows that while there was significant movement between the different villages in the Pontiac - resulting in the large cluster of points around Shawville - there was also significant movement from other parts of Ontario, Quebec, and even the United States. There were even visitors from as far as Edmonton (there was also data indicating connections to England and Nova Scotia, but Palladio didn't seem to connect those to my coordinates; these were marginal enough - only one mention each - that I kept the visualization as it was instead of spending more time playing with my data). I was surprised at how far some of the visitors travelled, but the overall concentration was still very local. It makes sense, furthermore, that a visitor from as far as Edmonton would get a mention in a small-town paper. More fascinating, I think, is how many mundane visits - from Quyon to Shawville and back again - were regularly recorded in the _Equity_.

This is especially true when I compare the visit data to the mentions of places throughout the _Equity_. The _Equity_ may have been overwhelmingly local in scope, but that was not out of ignorance or disinterest in world events. In fact, the map of locations mentioned in the _Equity_ spans the entire globe. Still, though, the concentration is in Shawville and Pontiac county, reflecting the primary use of the paper as a chronicle of local events. The second highest concentration was in England, probably reflecting the English language of the newspaper and Pontiac county - of course they were interested in British politics.

As I mentioned above, these "local events" were most often personal visits. I decided to network model the relationships between people and places in the January 1895 _Equity_. I loaded a .csv of the visits in this format: `place from, place to` into Gephi. I then ran the ForceAtlas2 algorithm to create an initial network. I calculated the PageRank - the relative "prestige" of each place, or how likely one area is to connect to any other - and sized the nodes based on relative PageRank. This created a network of locations based on how many people visited to or from that place.

Then, I added a second node type to my network by importing .csvs containing the information: `person, place visited` and `person, place lived`. This allowed me to visualize which residents may have been connected through their travels, even if they didn't necessarily visit each other. I organized these through the ForceAtlas2 layout, but I did not calculate PageRank, because I wasn't sure if the algorithm was designed for bimodal networks. I also decided not to create any edges between people in order to simplify my already-crowded network. Nor did I I also created a network connecting only people, without nodes for locations, but I found the bimodal network much more interesting. I coloured nodes for people orange and nodes for places green, resulting in this visualization:

Network model of visits in January 1895 _Equity_

The most fascinating part of this network, for me, is the centrality of Ottawa. Shawville has the most connections by far, but Ottawa is the connecting piece between many of the different regions. I wasn't expecting it, but it makes sense that Ottawa, as the biggest city in the region, would be a hub for travel. I was also surprised by the importance of Quyon; I wonder if this has to do more with the correspondant from Quyon (who reported the visits to the region in a letter to the _Equity_) was particularly attentive, or if Quyon was a central location in the region. I'm also interested in the chunks that jumped off of the main graph, although some seem to be mistakes (I noticed a node coloured orange but labelled "Quyon" in one of them, suggesting that they should be connected to the main Quyon node). It's interesting, for example, that Murrel's Settlement and Clarendon are off by themselves, and that Montreal is primarily connected to the network through Parkman. 

While this data is very narrow (covering only a month's worth of visits), it does suggest to me, again, the local scope of the _Equity_. Ottawa is important as a hub city, but other cities like Montreal and Toronto don't play a huge role in the network. Where social life was concerned, what's happening in Quyon and Shawville and Radford is much more important to the readers of the _Equity_. Local visits were newsworthy because they were relatively common - they sustained the community. Visits to or from larger cities were newsworthy for the opposite reason: they represented a departure from the local community, but they didn't happen nearly as often.

In summary:
   + While transcription provides a much more accurate corpus, the amount of time needed to transcribe enough data to see change over time using digital analysis makes relying on transcription, even partial transcription such as the events-focused transcription I performed on the January 1895 _Equity_, very inefficient.
   + The raw OCR of the _Equity_ is not ideal for extracting patterns related to social events, but it is sufficient. Removing headers and footers from articles and running spellchecks for common OCR mistakes would further improve its accuracy.
   + To see change in social activities over time, a range of longer than 10 years should be used. It seems trends in social habits did not shift very quickly in rural Pontiac, even at the end of the 19th century.
   + 1895 was generally a typical year for Pontiac county; no major abberations in social patterns seem evident. That being said, there were certain events, like the January carnival in Ottawa, that occured in 1895 and not in other years. Conversely, the drama that occured in February in later years did not occur in 1895.
   + Social events in Pontiac county were dependent on the season and time of year, probably relative to the rhythms of working life required by an agricultural community.
   + Most of the socializing in Pontiac county in 1895 was done not at organized public events, but through visits to relatives, friends, and the hosting of distinguished guests.
      + when organized public events did occur, they were usually related to religion, such as sermons by visiting clergymen, and municipal politics, such as meetings of town halls and school associations.
   + Visiting was most common just after the holiday seasons, especially after Christmas and Easter.
   + Semiprivate events such as concerts and dinner parties were also common after Christmas.
   + Women were mentioned often in relation to social visits, but less often in other contexts. This suggests that visiting was a way for women, including unmarried women, to interact with public life and be noticed by their community.
   + Local events carried more weight generally for readers of the "Equity" than international events. Local events were documented on the first page of the paper, and nearby localities were mentioned far more often than distant metropoles.
   + That being said, readers of the _Equity_ still had an interest in world events. This seems especially true in regards to British politics and the British empire.
   + Ottawa was important as the closest "city" in the region. Most settlements in Pontiac, and thus most people living in the Pontiac, had social ties to Ottawa.
   + Visits between regions in the Pontiac were far more frequently mentioned than visits to regions outside the Pontiac.
   + Some communties within the Pontiac, such as Murrel's Settlement and Clarendon, did not have much traffic with the rest of the county in January 1895. They still corresponded with the _Equity_, however, suggesting that a communal identification continued through the newspaper.
   + These local visits helped build and sustain a community. The _Equity_ represented this community by reporting on visits that may seem mundane to us. In a small community, however, any form of socializing was an important respite from day-to-day work and helped to sustain the region's unique identity and structure.
  
These conclusions are preliminary, but they illustrate a little of what social life was like in Shawville at the end of the 19th century. I discuss what further steps could be taken with the _Shawville Equity_ data in the section "Future work."

# Presenting data

As an English student, I'm very attached to narrative. One of my worries in taking this course was the thought that big history meant a necessary dilution of narrative. I learned quickly through posts such as Cameron Blevins' [Topic Modelling Martha Ballard's Diary](https://hyp.is/yY1rYGXPEee2uisRHn-0EQ/www.cameronblevins.org/posts/topic-modeling-martha-ballards-diary/) and Michelle Moravec's [Corpus Linguistics for Historians](http://historyinthecity.blogspot.ca/2013/12/corpus-linguistics-for-historians.html), that that wasn't necessarily true. From the first week of the course, then, I knew I wanted to find a way to present the social life of Shawville as a narrative.

I settled on a podcast for a few reasons. First, I'm a little bit obsessed with them (my greatest sacrifice for this project was not the hours of sleep lost, but the hours of saved podcasts deleted from my phone to make room for my recording). Secondly, I think a podcast is a good format for telling a complex narrative based around a sizeable community rather than a single cast of characters. There may be some interesting anthropological reason for that from our past as oral storytellers. I only have anecdotal evidence for it. I was thinking of podcasts like NPR's investigative jornalism series _Serial_ and _S-Town_, sprawling historical epics like _The British History Podcast_ and _The History of Japan Podcast_, and even _Welcome to Night Vale_, a dark comedy that relates the goings-on of a fictional desert town. All of these podcasts convey a LOT of information, but they are interesting and easy-to-follow all the same. Finally, I'm a tour guide, so I knew I'd be able to synthesize the information I gained from my analysis into a compelling oral presentation.

Originally, I wanted to do something akin to a non-fiction version of _Welcome to Night Vale_, but for Shawville in 1895. I would narrate the goings-on of Shawville residents over the course of one year and contextualize it within the social trends of 1890-1900. I planned four episodes, dividing the year into quarters: January to March, April to June, July to September, and October to December. I abandoned this idea when I realized how long the transcription process would take, and how many of the "social events" I did find evidence of were private visits rather than public events that could be shaped into a "community calendar."

My final project wound up being closer to a discussion of my findings, told through a loose narrative of social events from the 1895 _Equity_. I actually like this format; it's casual, direct, and easy-to-digest (unlike this massive block of text). To supplement my discussion, I uploaded my visualizations to a blog post hosted on my domain, labelled with a figure number and with a snippet of my podcast to contextualize the visualization. Ideally, people can listen to me chat, and follow along with the visualizations if they're interested.

I recorded my podcast on my iPhone SE. Not ideal, but after I realized I couldn't go through with my original "community calendar" idea, I was hesitant to do a podcast at all. By the time I settled on my final version I had to make do. I edited the audio in Audacity and supplemented it with open-licence music by X obtained from Y. I created a logo in Photoshop CS6 with a map of Shawville obtained from [Z]().

In the interest of accessibility, I also (transcribed)[] the final text of my talk, so that people who can't listen to a podcast can access my data as well.

I hosted my entire final project into a post on my blog, with a few listening options: a direct-download link, an embedded Soundcloud player, and an embedded player hosted on my domain.

# Future work

The following is a list of suggestions for future research using the _Equity_ into social life in late Victorian Pontiac county. I would love if my classmates from HIST3814O, or anyone who is interested in pursuing some of these topics, would contact me at [my Carleton University e-mail](mailto:sarahmcole@cmail.carleton.ca), or by commenting below.
   + A cleaner OCR of the _Equity_ would facilitate all kinds of future research. As OCR technology is always evolving, there's no reason why the .pdf files provided by BANQ could not be read into cleaner .txt files. My classmate [Jeff Blackadar]() did some interesting work using OCRfeeder in this vein.
   + A table similar to my dataset on January 1895 social events that spans a longer period of time would be excellent, particularly if it categorized the events by type and location within the paper.
   + [XML-encoded]() copies of historical texts will never not be useful. (If any of my classmates XML'd an issue of the _Equity_ for this project, let me know and I'll link your work here!)
   + The analysis methods I describe above may yield more interesting results if applied to a broader corpus of _Equity_ issues, perhaps spanning from 1890 to about 1920 or 1930; or even just over a decade in which social change occured more rapidly, like 1914-1924. I chose my years based on an interest in the late Victorian era and an unsubstantiated belief that the turn of the century might have accellerated changes in social patterns. Of course, I now see that social change didn't occur as quickly as I thought in 1890-1900, new century or not. Analyzing changes in social events around  more "watershed" periods, like 1914-1918 or 1929, might yield more interesting results.
   + Comparing the corpus of the local _Equity_ to a corpus of a more "national" newspaper, such as the _Globe and Mail_, from the same period would likely yield interesting information on the way community was represented and constructed by local newspapers. 
   + Similarly, comparing the corpus of the _Equity_ to a corpus of a local newspaper from a larger city, such as the _Ottawa Citizen_, from the same period could reveal differences in social ties in rural vs urban communities.
   + I didn't get a lot of time to do secondary research about newspaper culture in Canada in the Victorian era, but it would be worth analyzing the _Equity_ in the context of newswriting styles of the era. I found a lot of the events mentioned in the 1895 Equity were shocking - a woman who threw herself from a train! An 11-year old boy who attempted suicide! It's unlike anything we'd read today, and I would be interested to see how Shawville fits into relative trends regarding sensationalist reporting.
      + Sentiment analysis, despite [its shortcomings](https://annieswafford.wordpress.com/2015/03/02/syuzhet/), could be a fascinating method for this type of analysis.
   + The relationship between women and social visits could ne a great starting point for a gender-based analysis of the _Equity_, or for gender relations in rural communities. I would love to see a more detailed analysis of places in the _Equity_ that mention women. On a related note, I'd love to map out the relationship between women's role in the social network of Shawville and more institutional sources of power.
      + This would also be a good framework of analysis for newspapers from any historical era and region, really. I'm curious to see a network analysis of women mentioned by, say, writers of the French Enlightenment and French Revolution.
	  + It would also be interesting to see how mentions of women change in the _Equity_ in relation to the first- and second-wave feminist movements. It seems that sufferagettes of England hadn't quite made a splash in Shawville in the 1890s.
   + More mapping and network visualizations of visits across a wider timeframe would provide an idea of how people moved and travelled in rural 19th-century Canada. Sentiment analysis of articles describing travel could reveal how people viewed long- and short-distance travel. Such work could also reveal the relationships between small towns like Shawville and surrounding cities like Ottawa, Montreal, and Toronto.
   + Finally, the entire framework of a 19th-century newspaper as a "proto-social network" is fascinating to me. If I had more time, I might have tried to reformat text from the _Equity_ as a social media platform: how can transforming the presentation of data influence how we see the data? Would we see more clearly how connected the world was, even back in the 19th century, or would we simply [project our own ideas of social networks onto the past](http://www.jstor.org/stable/10.1525/rep.2014.127.1.64?seq=1#page_scan_tab_contents)? There are many other ways we could explore the connection between community and print culture; digital history methods wold be uniquely suited to these sorts of projects because digital historians are already used to challenging the assumed dichotomy between the past and modern technology.

# Personal reflection

I'm going to try not to wax too poetic here. Partly because I'm exhausted, and partly because I'm proud. I hope my work shows how much I learned from this class. I also hope it shows how much I enjoyed it.

I had difficulty with some of the exercises in this class, obviously; all of that is detailed in my various faillogs. The most difficult thing, though, was approaching my final project. I was petrified when I started out. I've written long research papers before, but those aren't quite the same as a digital history project. When I wrote my papers, I was in a safe space: I knew the methodology, I knew the topic, I knew what was expected of me. When Dr. Graham told me to construct my own project from scratch - and to construct it using research methods I was only just becoming familiar with - was terrifying.

It was especially terrifying because before I took this class, I didn't exactly share Dr. Graham's philosophy of "failing productively." I knew that technically (you miss 100% of the shots you never take, etc etc), but practically, I had always been afraid of failing in an academic setting. I was a perfectionist. Well, I still am - seven weeks isn't going to fix that - but now I can see the value of just _doing something_, even if I'm not sure how it's going to turn out. That's the only way I was able to get this project done.

The best example of that was probably my aborted attempt at transcribing all social events in the 1895 _Equity_. My original plan was pretty reliant on the idea that I'd be able to whip up a searchable transcription in about a night's work. After six hours at my computer screen, propped on my couch listening to my sixth or seventh straight episode of _My Favourite Murder_, I had only finished January. I exchanged a few messages with Dr. Graham in which I tried to mask my absolute panic. My project was falling apart before my eyes.

But I reached out, I asked for help, and Dr. Graham suggested comparing my transcribed data to the wider corpus. That shaped my entire project, and made my workload a lot more manageable. Admitting failure led to my eventual success.

I do think this project is a success. I'm quite proud of it, 
actually, even though the recording was made in a frantic, Lays 
potato chip-fueled 24 hours. I can't believe I gleaned this much 
information from the _Equity_ OCR that so terrified me when I first 
openend it. I can't believe I produced this much in just seven weeks. I can't believe how much I liked doing it. I _especially_ can't believe I actually wrote and ran TWO (2!) python programs (well, one was copied and pasted from [Fred Gibbs](fredgibbs.net), but I _understood what I was doing with it_).

I still have a long way to go as a digital historian/digital humanist. I would like to have found something more punchy in my analysis of the _Equity_, beyond just "hey, even back in the olden days people went and visited their friends and stuff." As exciting as the process was, the findings aren't super exciting to me. Within my limited time frame, though, I think I unearthed some interesting patterns. I definitely had my opinion of the _Equity_ changed. I thought it would be boring to look through the newspapers of a rural town. Turns out, almost anything can be fun to explore, if you have the tools to do it. And my toolkit's not complete, but it's bigger than it was before.

Now that I have these tools, I'd love to apply them to my other passion: English literature. I'm super interested in comparing the poetry of Edna St. Vincent Millay to the work of the modernist poets, from whose high pantheon she is usually excluded. How different was her work, really, from the modernists? Can digital tools trace similarities between her work and that of other poets? How does her corpus compare to the corpus of female modernists like Katherine Mansfield and Virginia Woolf? I'm already thinking about how I can mark up Millay's poems with XML and where I might scrounge up a corpus of T. S. Eliot's work for comparison. As I've been working full-time and taking a full courseload this summer, these are currently just ideas; but they're ideas that excite me more and more.

I've even come to think of doing more digital history. My discovery 
of the gender disparity relating to social events in the _Equity_ 
made me wonder if text analysis could be used to track women's roles 
in creating social groups that produced great bodies of literature - 
I'm thinking, in particular, of the women who ran _salons_ in 18th-century France. They were mentioned occasionally in letters and diaries of the men that they hosted; I bet a network model linking them to male philosophers, politicians, and writers would reveal something about female influence in the French Revolution.

Or maybe it wouldn't. Maybe the project would fail completely. I'm learning to be okay with that, though, thanks to this class. I've enjoyed working in the supportive community fostered on the class' Slack page, and I'll honestly miss checking it. Having a community, I've learned, softens the fall. I hope I can keep up with the DH community in some way, even if I don't maintain a digital history blog. The passion with which digital historians engage with the public and with their own personal projects inspires me. It's a lot of work, but being open with research is motivating. I'm definitely considering incorporating an open, digital element into my Masters work, regardless of whether or not I take a full Masters in DH.

I'll admit, though, I'm looking forward to resting my mind for a bit. Digital history and  its methods can drive me a bit mad, I've found. I'm already dreaming of XMLing all my notes next semester. I'm glad to at least have found a productive kind of madness.