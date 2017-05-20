---
layout: post
title: Search Engine
---
In the fall of 2016, I wrote and implemented a search engine as a part of a class on search informatics at Indiana University (INFO-I 427). This project leveraged several technologies:

* Python
* Twitter Bootstrap

This project is made of several different parts:

* Crawler

Description: Crawl reads in a user-inputted url, number, and folder to begin scraping. After initializing itself with not only the command line inputs, but also the rules, start_urls, num, directory, and finally creating the directory where all the html files will be kept. The start_url we just assigned with our user inputs begins the crawl in parse_url. Parse_url takes a spider and an http request response, and creates an instance of an item in our spider with it's full html body copied into item['html'] - which will later be transferred to a file and cleared - the url of the page the spider is currently crawling, and then extracts all the links from the page and assigns a list version of all the urls to the item to then go on to be processed, but not before we decrease the check to see if self.num is at zero, as to keep track of how many pages we have successfully crawled and know when our spider should stop. From there we head over to pipelines.py to process the information before we make any further decisions on whether or not to keep it. The class DuplicatesPipline does two major things: it gets rid of any of our spider items that are already in existence (and then increases the num by one, so that we can make sure we're keeping the number of pages we have successfully crawled and logged true) and then if it is not a duplicate, this pipeline creates an html file that is a copy of the item['html'], then clears out the item['html'] just to save a little space, adds the filename along with the page's url to the index.dat file, adds that filename to the item['filename'], and then finally returns the item. From there, we enter the FilterAndWritePipline, where we add our filename and url to a little dictionary called matches and append our item onto the list of all items within the spider while the spider is still open, and then upon the signalling that the spider is closed, this pipeline assembles the dictionary for our graph.dat file that reads in a dictionary, that has been converted into string format, of filenames of pages that have been crawled as keys and all of the filenames of the links that have been crawled off of their item['links'] list.


* Indexer

Description: Index takes a directory of html pages and a dat file of the names of the html files and the URLs at which the files can be found. It first parses the html of each page with beautiful soup, then takes out all stopwords and stems all remaining tokens. Index also creates bigrams for all stemmed tokens. At this point, Index outputs to two dat files, docs.dat and invindex.dat. In invindex, Index places a dictionary of all terms, both unigrams and bigrams, with the corresponding names of the pages on which they can be found and the number of occurances of the term in the page. In docs, Index places a dictionary of all of the names of the html files and their corresponding amount of tokens, webpage titles, and URLs.

* Link Analysis

Description: Pagerank takes the graph.dat and outputs a dictionary to the pagerank.dat. This dictionary contains keys, whose values are their pagerank values. The pagerank values are based on a recursive algorithm. It starts with a damping factor, which serves to lessen the effects of the values on which pagerank is based for further iterations. One minus this is added to the product of the damping factor and the pagerank of all of the pages that link to the page divided by the number of all of the pages to which that page links. This allows a search engine to determine which pages are the most commonly visited.

* Retrieval and Ranking 

Description: Retrieve takes the inverted index and queries it for search terms. These search terms are rid of stopwords and stemmed. The or mode inclusively returns dictionary key values for each search term. The and mode exclusively returns dictionary key values for all search terms. The most mode returns dictionary key values if most (half or more) of the search terms appear in the documents. The printed values from the dictionary in docs.dat are the url and webpage title of each of the documents.

My search engine implementation, based on Killer Whales, can be found [here](http://cgi.soic.indiana.edu/~ndleisur/).