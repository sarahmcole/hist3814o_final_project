# HIST3814O PROJECT: A CSV OF DATES, 1895
## AUGUST 3 2017
### SEE FAILLOG_FINALPROJECT.MD FOR INFORMATION

    1  history -w
    2  cd equity
    3  mkdir 1895
    4  cd 1895
    5  sed -r -i.bak 's/(.+[A-Za-z ]day.+)/~\1/g' *.txt
    6  grep '~' *.txt > dates.txt
    7  nano dates.txt
    8  sed -r -i.bak 's/83471_//g' dates.txt
    9  sed -r -i.bak 's/:~//g' dates.txt
   10  nano dates.txt
   11  sed -r -i.bak 's/.txt/ /g' dates.txt
   12  nano dates.txt
   13  mv dates.txt.bak dates.txt
   14  nano dates.txt
   15  sed -r -i.bak 's/.txt/, /g' dates.txt
   16  mv dates.txt.bak dates.txt
   17  nano dates.txt
   18  sed -r -i.bak 's/,//g' dates.txt
   19  sed -r -i.bak 's/.txt/, /g' dates.txt
   20  nano dates.txt
   21  sed -r -i.bak 's/([0-9(.+)([A-Za-z ]day)
   22  sed -r -i.bak 's/(.+),(.+)([A-Za-z ]day)(.+)/\1, \3, \2\3\4/g' dates.txt
   23  nano dates.txt
   24  mv dates.txt.bak dates.txt
   25  sed -r -i.bak 's/(.+),(.+)([A-Za-z ]{1,6}day)(.+)/\1, \3, \2\3\4/g' dates.txt
   26  nano dates.txt
   27  mv dates.txt.bak dates.txt
   28  nano dates.txt
   29  sed -r -i.bak 's/(.+),(.+)(\b.+[A-Za-z ]day)(.+)/\1, \3, \2\3\4/g' dates.txt
   30  nano dates.txt
   31  grep '[^day]' dates.txt
   32  nano dates.txt
   33  grep -L "day" dates.txt
   34  grep ack -L day dates.txt
   35  grep ack -L 'day' dates.txt
   36  grep -L 'day' dates.txt
   37  dates.txt
   38  nano dates.txt
   39  grep ', :' dates.txt
   40  sed -r -i.bak 's/.+, :.+//g' dates.txt
   41  nano dates.txt
   42  grep '[^[:blank:]]' dates.txt > dates_cleaned.txt
   43  nano dates_cleaned.txt
   44  cp dates_cleaned.txt dates_cleaned.csv
   45  nano dates_cleaned.csv
   46  nano dates_cleaned.csv
   47  history > project_csvdates1895.md
