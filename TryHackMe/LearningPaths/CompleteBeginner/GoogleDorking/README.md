# Google Dorking Room

Link: https://tryhackme.com/room/googledorking

## [Task 1] Ye Ol' Search Engine
No questions to answer - just click to confirm

## [Task 2] Let's Learn About Crawlers
This section explains what Web Crawlers are and how they work.

### Questions
1. Name the key term of what a "Crawler" is used to do
> index

2. What is the name of the technique that "Search Engines" use to retrieve this information about websites?
> crawling

3. What is an example of the type of contents that could be gathered from a website?
> keywords

## [Task 3] Enter: Search Engine Optimisation
This section describes how search engines will rank domains that are easier to index. It discusses how search engines calculate those rankings.

### Questions
> Note: I did this 25th September 2020. It is possible the answers may change.

1. Using the SEO Site Checkup tool on "tryhackme.com", does TryHackMe pass the “Meta Title Test”? (Yea / Nay)
> Yea

2. Does "tryhackme.com" pass the “Keywords Usage Test?” (Yea / Nay)
> Nay

3. Use https://neilpatel.com/seo-analyzer/ to analyse http://googledorking.cmnatic.co.uk
> No answer needed

4. With the same tool and domain in Question #3 (previous): How many pages use “flash”
> 0

5. From a "rating score" perspective alone, what website would list first - tryhackme.com or googledorking.cmnatic.co.uk? (Use tryhackme.com's score of 62/100 as of 31/03/2020 for this question)
> googledorking.cmnatic.co.uk

## [Task 4] Beepboop - Robots.txt
This section discusses the robots.txt file that domain admins can put on websites to direct crawlers as to what kind of permissions they have (e.g. what pages can and cannot be scraped)

### Questions
1. Where would "robots.txt" be located on the domain "**ablog.com**"
> ablog.com/robots.txt

2. If a website was to have a sitemap, where would that be located?
> /sitemap.xml

3. How would we only allow "Bingbot" to index the website?
> User-agent: Bingbot

4. How would we prevent a "Crawler" from indexing the directory "/dont-index-me/"?
> Disallow: /dont-index-me/

5. What is the extension of a Unix/Linux system configuration file that we might want to hide from "Crawlers"?
> .conf

## [Task 5] Sitemaps
Discusses sitemap files which outline the structure of a website to make it easier for search engine crawlers to scrape

### Questions
1. What is the typical file structure of a "Sitemap"?
> XML

2. What real life example can "Sitemaps" be compared to?
> map

3. Name the keyword for the path taken for content on a website
> route

## [Task 6] What is Google Dorking?
Discusses the advanced search options of Google.

### Questions
1. What would be the format used to query the site bbc.co.uk about flood defences
> site: bbc.co.uk flood defences

2. What term would you use to search by file type?
> filetype:

3. What term can we use to look for login pages?
> intitle: login
