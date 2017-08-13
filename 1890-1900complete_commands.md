# HIST3814O FINAL PROJECT
## August 12 2016
### The great .csv creation

My goal: create .csv files of Equity for 1895 and for 1890-1900 in the following format: `YYYY-MM-DD, text of issue`.

    1  history -w
    2  cd equity
    3  cd 1895
	
Concatenate files into one file.

    4  cat *.txt > 1895complete.txt
    5  nano1895complete.txt
    6  nano 1895complete.txt
    7  cd equity
    8  cd 1895

I knew ###PAGE###1### would be used only once per issue, so greping it would give me a grep output that I could then turn into a list of YYYY-MM-DD issue dates (because the title of each .txt file contains the YYYY-MM-DD of each issue by default.)
	
    9  grep "### PAGE 1 ###" > equity1895table.txt
   10  grep "###PAGE###1###" *.txt
   11  grep "###PAGE###1###" *.txt > equity_1895_table.txt
   12  sed -r -i.bak 's/83471_//g' equity_1895_table.txt
   13  sed -r -i.bak 's/.txt:###PAGE###1###/,/g' equity_1895_table.txt
   14  nano equity_1895_table.csv
   15  nano equity_1895_table.txt
   
I then realized that I should focus on getting the text of each issue into a single line first... so I backtrack.
   
   16  cd 1895complete

Get rid of pesky `,` and `"` that would ruin my formatting.
   
   17  sed -r -i.bak 's/,|\"//g' 1895complete.txt
   18  sed -r -i.bak 's/###PAGE###1###/,/g' 1895complete.txt
   19  grep "," 1895complete.txt
   
I got this pattern for removing all line breaks off stackexchange.
   
   20  sed -i.bak -i ':a;N;$!ba;s/\n/\t/g' 1895complete.txt
   
The following file has one line with `,` at the beginning of each issue (where `###PAGE###1###` was)
   
   21  nano 1895complete.txt

Then I put each issue onto a new line.

   22  sed -r -i.bak 's/,///n,/g'
   23  sed -r -i.bak 's/,/\\\n,/g' 1895complete.txt

Then I tried to add in the YYYY-MM-DD using grep, but I evidently forgot wtf I was doing a bunch of times
   
   24  nano 1895complete.txt
   25  grep ',' 1895complete.txt > 1895completetable.txt
   26  nano 1895completetable.txt
   27  grep "," 1895complete.txt > 1895complete_table.txt
   28  nano 1895complete_table.txt
   29  grep "," 1895complete.txt
   30  1895complete.txt
   31  nano 1895complete.txt
   32  nano 1895complete.txt
   33  grep "," 1895complete.txt > 1895_complete_table.txt
   34  1895_complete_table.txt
   35  nano 1895_complete_table.txt
   36  grep -a "," 1895complete.txt > 1895complete_table.txt
   37  nano 1895complete_table.txt
   
I finished turning 1895complete_table.txt into a well-formed .csv in Notepad++.   
   
I repeated this basic process later much more efficiently for the corpus from 1890-1900. Yay, improvement.

   56  cd equity
   57  cd 1895
   58  cd
   59  cd 1895
   60  cd 1895complete
   61  nano 1895complete_table.txt
   62  mv 1895complete_table.txt 1895complete_table.csv
   63  nano1895complete_table.csv
   64  nano 1895complete_table.csv
   65  sed -r -i.bak 's/,//' 1895complete.txt
   66  nano 1895complete.txt
   67  cd
   68  cd equity
   69  cat *.txt 1890-1900_complete.txt
   70  cat *.txt > 1890-1900_complete.txt
   71  grep "###PAGE###1###" 1890-1900_complete.txt > 1890-1900_issues.txt
   72  nano 1890-1900_complete.txt
   73  sed -i.bak -i ':a;N;$!ba;s/\n/\t/g' 1895complete.txt
   74  sed -i.bak -i ':a;N;$!ba;s/\n/\t/g' 1890-1900_complete.txt
   75  nano 1890-1900_complete.txt
   76  sed s'###PAGE###1###/\\\n/g' 1890-1900_complete.txt
   77  sed -r -i.bak s'###PAGE###1###/\\\n/g' 1890-1900_complete.txt
   78  sed -r -i.bak 's/###PAGE###1###/\\\n/g' 1890-1900_complete.txt
   79  nano 1890-1900_complete.txt
   80  grep "###PAGE###1###" *.txt > 1890-1900_issues.txt
   81  sed -r -i.bak 's/83471_//g' 1890-1900_issues.txt
   82  sed -r -i.bak 's/".txt:.+//g' 1890-1900_issues.txt
   83  nano 1890-1900_issues.txt
   84  sed -r -i.bak 's/###PAGE###1###//g' 1890-1900_issues.tt
   85  sed -r -i.bak 's/###PAGE###1###//g' 1890-1900_issues.txt
   86  history > 1890-1900complete_commands.md
