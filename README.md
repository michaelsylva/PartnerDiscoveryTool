# Partner Discovery Tool

The purpose is to identify partner companies and clients mentioned on the provided website scrapes.
Additionally, the script attempts to find logos within the HTML content and categorizes them as either
related to clients, partners, or unknown based on the text around the image. Images that are likely to
be a logo are processed through Google Vision to determine a company name. The final output
includes a merged output of all the company names, occurrences on the site, and a label ‘partner’ or
‘client’ or ‘unknown’ for each.

The script is pulling the scrapes from the diffbot_crawl_data table and updating a new column called "partner_data" with the result.

## Functionality

### 1. Text - Named Entity Recognition (NER)
The transformers library is used to perform NER, specifically looking for organizations (companies)
in the provided HTML text content.
The text on each site is searched for entities tagged as a company name, for each company name
found it ensures that it did not find the company name of the site. The list is then iterated over to
check context looking for key terms like ‘client’ or ‘partner’ or ‘customer’, etc and each company name
is labeled as ‘partner’, ‘client’ or ‘unknown’.

### 2. Logo Identification & Google Vision
The script extracts images from HTML content and attempts to identify logos. Logos are categorized
based on their proximity to certain keywords and phrases in the HTML content or based on other
various factors such as the page name. The URL and alt tags are searched for anything related to
logos. Logo labels include 'client,' 'partner,' or 'unknown.' Each logo URL is then requested through a
realistic user-agent and then processed to ensure it is a valid image to optimize the amount of Google
Vision requests.

### 3. List Validation
Any duplicates found between the lists at step 1 & 2 are merged to a final list filtering out common
company names, such as TikTok, Twitter, Facebook, etc

## Installation
1. Extract the zip `PartnerDiscoveryTool.zip `. 
2. Ensure there is an installation of Python 3.11 and create a new environment for the requirements (recommended)
3. Install requirements with `pip install -r requirements` 
4. Setup database configuration options in `config.json`.
5. Follow this guide: https://cloud.google.com/docs/authentication/gcloud installing `gcloud-cli` on the server that will be executing the script. Authenticate with a Google Cloud Project ID with billing information connected.
6. When you want to extract partners from the database, run main.py (expected to be setup on a scheduled interval). The script will run according to DB information in config.json

## Configuration
### Parameters
Sensitivity Parameters:
- sensitivity: Adjusts the sensitivity for distance away from a key term in text for
identifying company names. Default: 50
- image_sensitivity: Adjusts the sensitivity for distance away from a key term for logo
identification based on image proximity to certain keywords. Default: 25
- google_vision_threshold: Adjusts the sensitivity for where Google Vision scores are
acceptable. Default: 0.79

Common Companies:
- The commons list contains common company names to filter out.

## Considerations
### Named Entity Recognition (NER):
- The script uses a pre-trained model for NER, which may not cover all possible organizations.
- Customization of NER models may be necessary based on specific requirements.
### Logo Identification:
- The logo identification logic relies on heuristics and may require adjustments based on specific website structures.
### Performance:
- The script processes HTML content and images, calls Google Vision and requests web
pages which may impact performance based on the number and size of websites.


The logos that are identified from the previous section are sent to Google Vision to retrieve possible
logo identifications. The script will find the highest score result and make sure it is greater than the
google_vision_threshold configuration option.

## Conclusion
This Python script serves as a tool for extracting and analyzing information from HTML content,
focusing on identifying partner companies and clients. It utilizes a combination of NER, image
analysis, and heuristic approaches. Continuous testing and adjustment are recommended for optimal
performance in different scenarios.
