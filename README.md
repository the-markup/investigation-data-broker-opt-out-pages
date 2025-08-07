# How we found data brokers hiding their data deletion instructions from search engines

This repo contains data for our story ["We caught companies making it harder to delete your personal data online"](https://themarkup.org/privacy/2025/08/12/we-caught-companies-making-it-harder-to-delete-your-data).

## Methodology

The Markup and CalMatters downloaded a copy of the California Privacy Protection Agency's [2025 Data Broker Registry](https://cppa.ca.gov/data_broker_registry/). The registry contains data the brokers are required to report to California on a yearly basis. This data includes a link to the page on the broker's website that instructs you how to, as [the registry index says](https://cppa.ca.gov/data_broker_registry), "delete your personal information and exercise your [CCPA privacy rights](https://cppa.ca.gov/faq.html)."

We extracted 499 unique links from the data file and wrote a script to visit each one, looking at three common methods for hiding the existence or content of web pages from search engines: the [robots meta tag](https://developers.google.com/search/docs/crawling-indexing/robots-meta-tag#robotsmeta), the [X-Robots-Tag HTTP header](https://developers.google.com/search/docs/crawling-indexing/robots-meta-tag#xrobotstag), and the [robots.txt file](https://developers.google.com/search/docs/crawling-indexing/robots/robots_txt). Through this process, we discovered 35 data broker sites hiding pages from search engines: 34 that used robots meta tags, and one that used a robots.txt file.

In the course of our investigation, we made adjustments to some URLs in California's registry. For 64 sites, we removed `www.` from the beginning of the domain name to resolve server errors. For an additional 16 sites, we manually edited the URL to get to a working page. In some cases, we fixed a typo, in others, we visited the broker's main site and found the correct link by navigating to it.

We excluded some URLs from our count that used a robots meta tag to hide a page; we did this when the link was to a file hosted on another site (for example, a PDF on box.com or a Google Doc) or to a third-party opt-out service.

## Data

For each of the 35 sites we found hiding their opt-out or privacy pages from search engines, we saved screenshots and [HAR files](https://en.wikipedia.org/wiki/HAR_(file_format)), which document network activity. HAR files can be read [in many modern browsers](https://help.tanium.com/bundle/HARfile-Create-Read/page/KA/HARfile-Create-Read/HARfile-Create-Read.htm) and with [the HAR Analyzer tool](https://toolbox.googleapps.com/apps/har_analyzer/). The files can be found in the [data/artifacts/](data/artifacts/) folder. We captured HAR files and screenshots from the Internet Archive for seven sites, as they'd removed the robots meta tag from their page during the course of our reporting.

We are also including a CSV file that records the outcome of our tests for all 499 URLs found in the registry; see the details below.

## Data Dictionary

* **[data/data-broker-opt-out-pages.csv](data/data-broker-opt-out-pages.csv)** 139KB. 500 rows. First row is the header.

| Field | Description |
| --- | --- |
| order_original | The order this record appeared in the original download |
| company_name | The name of the company |
| hostname_original | The hostname of `url_original` |
| hiding_found | TRUE if the site used one of the technologies we investigated to hide a page from search engines |
| hiding_stopped_confirmed_at | The date we confirmed that the site had stopped hiding a page from search engines, if they did so before we published our investigation |
| url_original | The page URL from the original download |
| url_checked | The URL that was checked to see if the page was being hidden |
| url_final | The final URL that was rendered by the browser |
| url_edited | TRUE if we edited `url_original` when it didn't resolve |
| server_redirected | TRUE if `url_final` is different from `url_checked`, meaning that there was a server redirect |
| result_confirmed | TRUE if we checked our script's finding manually |
| meta_tag | The content of the robots meta tag, if it was found |
| x_robots_tag | The content of the X-Robots-Tag HTTP header, if it was found |
| robots_txt_restricts | TRUE if the server's robots.txt file restricted search engines' access to the page |
| status | The status of the web request to `url_checked` |
| error_status_details | More detail on `error` statuses |
| collects_minors_data | TRUE if the data broker told the state that they collect the personal information of minors |
| collects_location_data | TRUE if the data broker told the state that they collect individuals' location data |
| collects_reproductive_data | TRUE if the data broker told the state that they collect individuals' reproductive health care data |
| other_company_names_that_use_url_original | If more than one company uses the same `url_original`, they are listed here |

## License

Copyright 2025 CalMatters / The Markup

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
