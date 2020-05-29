
+++
date = "2013-08-20 05:31:00+00:00"
draft = false
tags = ["csv", "python", "tip"]
title = "avoid accessing wrong column in csv"
url = "/post/58766042678/avoid-accessing-wrong-column-in-csv"
+++
_Avoid accessing wrong column in csv_

I was parsing few csv files. All were from same provider. Format is`` Date, Name, Email, Company, City `` etc in one file. I made an assumption all the downloaded files are in the same format. For my surprise few files had same format and others din’t.

    with open(filename, 'rb') as f:
        reader = csv.reader(f)
        reader.next() # first row contains column names
        for row in reader:
            name, email, company = row[1], row[2], row[3]
            #save to db

In the above code the fundamental mistake is fixing the position of the columns in csv file. What happens if `` email `` and `` company `` position is interchanged? In simple `` screwed ``.

_How can I avoid these situations ?_

     with open(filename, 'rb') as f:
         reader = csv.reader(f)
         header = reader.next() #reader is not list
         name_pos, email_pos, company_pos =          header.index("Name"),header.index("Email"),header.index("Company")
    
     with open(filename, 'rb') as f:
         reader = csv.reader(f)
         reader.next()
         for row in reader:
             name, email, company = row[name_pos], row[email_pos], row[company_pos]
             #save to db

The code is bit longer here, but robost. The above snippet doesn’t rely on column position but only relies on column name in the first line. Ofcourse `` header.index `` will raise `` ValueError `` you can wrap the exception. First all the required column positions are retrieved, file is reopened, first row is omitted and other details are accessed.
