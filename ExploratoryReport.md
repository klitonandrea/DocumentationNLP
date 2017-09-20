# Data exploration for text analysis report

## Problem definition

MuleSoft products [documentation](https://docs.mulesoft.com) is categorized by each product that is developed in the company. It is hard to navigate through the documentation as the number of products has grown. As a result, the experience of a MuleSoft product user coming to this website for the first time is not the best and could be improved.  The interested party (client) in this case is Product Management team. The client would benefit from restructuring the documentation as part of the product. 

The documentation structure could be improved by identifying common topics in documents (web pages).  This goal can be achieved by leveraging such state of the art techniques like LDA.  Once the topics are identified the structure of the documentation could be improved by re-grouping the documents in a comprehensive way. Helping the user to spend less time finding the information of interest would make the documentation site more attractive and as a result convert the leads to become customers.
Identifying similar topics between the documents would also result in providing more relevant references inside the documentation.


## Dataset, data cleaning

The data to analyze are located here: https://github.com/mulesoft/mulesoft-docs

The contents are written in AsciiDoc format (http://asciidoc.org/). The .adoc files are text files with specific tags. The tags will be handled in the preprocessing phase. The links to the graphics won’t  be considered and should get removed. The code snippets will be replaced with ‘code’ token. 

The zipped bundle contains all the versions of the product in separate folders. In this project we will review only the latest version of documented products. The previous versions do differ but not that much in the context of the goal we want to achieve. Several versions of the same product create redundancy in the input data. Which does bias the text analyses. Leaving only the latest version texts is performed manually.

The image and attachment folders are also removed from our dataset. These folders contain non textual information. Also other configuration files (.yml files) and table of contents in the folders are excluded.

Each file is treated as document. The files are loaded into a pandas Dataframe object. For each document:
	- remove code snippets defined in asciidoc by the following regex: r"\n\[(\w[,-.\s]*)+\]\n----.*?----"
	- remove links to web pages but extracting the text: r"link:.*?\[(\w*)\]*?"
	- remove image links:  r"image:.*?\[(\w([\s.-])*)*\]*?"
	- remove any other non alpha numeric character together with line feeds, etc.
	- convert the text to lower case

After the above processing we have only text representation of the content that we can work with. Still there will be text related to xml tags, configurations, tables and java packages. Specific tokens will get filtered out if not relevant.

## Initial findings