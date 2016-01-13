---
title: ProPublica Campaign Finance API Reference

language_tabs:
  - shell
  - ruby
  - python

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Using the Campaign Finance API, you can retrieve data from United States [Federal Election Commission](http://www.fec.gov/disclosure.shtml) filings and other sources. The API, which [originated at The New York Times](http://open.blogs.nytimes.com/2008/10/14/announcing-the-new-york-times-campaign-finance-api/) in 2008, covers summary information for candidates and committees, as well as certain types of itemized data. This document describes the requests that users can make of the API and the responses that it returns.

<aside class="notice">In this document, curly braces { } indicate required variables. Square brackets [ ] indicate optional parameters or placeholders.</aside>

### Scope of data

* Candidate and committee data: 1980–present
* Committee contributions: 1987/88–present
* Congressional summary financial data: 2000–present (even-numbered years)
* Presidential campaign data: 2008–present (even-numbered years)
* Electronic filings: 2001–present
* Paper filings: 1999–present
* Independent Expenditures: 2009–present
* 48-hour notices of contributions/loans received: 2010-present
* Lobbyist bundler data: 2012-present

The ProPublica Campaign Finance database is updated twice daily (electronic filings are updated every 15 minutes). For general information about FEC data, visit the [FEC Web site](http://www.fec.gov/disclosure.shtml).

The data returned by the Campaign Finance API is subject to the [official sale and use restrictions](http://www.fec.gov/pages/brochures/sale_and_use_brochure.pdf) set by the Federal Election Commission. By accessing this data via the ProPublica API, you understand and acknowledge that you are using the data subject to all applicable local, state and federal law, including the FEC restrictions.


# Authentication

> To authorize, use this code:

```ruby
require 'campaign_cash'

CampaignCash::Base.api_key = YOUR_API_KEY

```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: YOUR_API_KEY"
```

> Make sure to replace `YOUR_API_KEY` with your API key.

To use the Campaign Finance API, you must sign up for an API key. Usage is limited to 5000 requests per day (rate limits are subject to change). The API key must be included in all API requests to the server, as a query string parameter:

`Authorization: YOUR_API_KEY`

<aside class="notice">
You must replace <code>YOUR_API_KEY</code> with your personal API key.
</aside>

# Requests

The Campaign Finance API uses a RESTful style. See individual methods below for permitted requests. The API only accepts GET requests. All requests begin with:

`https://api.propublica.org/svc/elections/us/{version}/finances`

The current version is `v1`.

## Common Parameters

These parameters are used in all request types. See below for URI structures and additional parameters for each type of request.

  * *version* The API version (in URI path). The current version is `v1`.
  * *campaign-cycle* An even-numbered year between 1996 and 2016 (in URI path). The current cycle in `2016`.
  * *api-key* Your ProPublica API key (in query string).

The following parameters are optional:  

  * *callback* JSONP callback function (in query string).

# Candidates

## Search for Candidates

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
  "status": "OK", "copyright": "Copyright (c) 2016 Pro Publica Inc. All Rights Reserved.", "cycle": 2016,
  "base_uri": "http://api.propublica.org/svc/elections/us/v3/finances/2016/", "num_results": 2, "offset": null,
  "results": [{"candidate": {"id":"H8IN07184","relative_uri":"/candidates/H8IN07184.json","name":"CARSON, ANDRE","party":"DEM"},"committee":"/committees/C00442921.json","state":"/seats/IN.json","district":"/seats/IN/house/07.json"},
  {"candidate":{"id": "P60005915","relative_uri": "/candidates/P60005915.json","name": "CARSON, BENJAMIN S SR MD", "party": "REP"},"committee":"/committees/C00573519.json","state":null,"district":null}]
}
```

This endpoint has two

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/candidates/search`

### Query Parameters

Parameter | Description
--------- | -----------
query | The first or last name of the candidate


## Get a Specific Candidate

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"http://api.propublica.org/svc/elections/us/v3/finances/2016/",
   "cycle":2016,
   "results":[
      {
         "id":"H4WA05077",
         "name":"MCMORRIS RODGERS, CATHY",
         "party":"REP",
         "district":"/seats/WA/house/05.json",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?H4WA05077",
         "committee":"/committees/C00390476.json",
         "state":"/seats/WA.json",
         "mailing_address":"32 EAST 25TH",
         "mailing_city":"SPOKANE",
         "mailing_state":"WA",
         "mailing_zip":"99203",
         "status":"I",
         "total_receipts":1184618.0,
         "total_from_individuals":464512.0,
         "total_from_pacs":524880.0,
         "total_contributions":989392.0,
         "candidate_loans":0.0,
         "total_disbursements":1003888.0,
         "begin_cash":182754.0,
         "end_cash":363484.0,
         "total_refunds":5300.0,
         "debts_owed":142194.0,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30",
         "independent_expenditures":0.0,
         "coordinated_expenditures":0.0,
         "other_cycles":[
            {
               "cycle":{
                  "id":5295,
                  "fecid":"H4WA05077",
                  "name":"MCMORRIS RODGERS, CATHY",
                  "party":"REP",
                  "party3":null,
                  "seat":null,
                  "status":"I",
                  "address_one":"32 EAST 25TH",
                  "address_two":"",
                  "city":"SPOKANE",
                  "state":"WA",
                  "zip":"99203",
                  "fec_committee_id":"C00390476",
                  "cycle":2014,
                  "district":"05",
                  "created_at":null,
                  "updated_at":null,
                  "office_state":"WA",
                  "cand_status":"C",
                  "branch":"H",
                  "current":null
               },
               "relative_uri":"/candidates/H4WA05077.json"
            },
            {
               "cycle":{
                  "id":8056,
                  "fecid":"H4WA05077",
                  "name":"RODGERS, CATHY MCMORRIS",
                  "party":"REP",
                  "party3":null,
                  "seat":null,
                  "status":"I",
                  "address_one":"32 EAST 25TH",
                  "address_two":"",
                  "city":"SPOKANE",
                  "state":"WA",
                  "zip":"99203",
                  "fec_committee_id":"C00390476",
                  "cycle":2012,
                  "district":"05",
                  "created_at":null,
                  "updated_at":null,
                  "office_state":"WA",
                  "cand_status":"C",
                  "branch":"H",
                  "current":null
               },
               "relative_uri":"/candidates/H4WA05077.json"
            },
            {
               "cycle":{
                  "id":10769,
                  "fecid":"H4WA05077",
                  "name":"MCMORRIS, CATHY",
                  "party":"REP",
                  "party3":null,
                  "seat":null,
                  "status":"I",
                  "address_one":"P.O. BOX 137",
                  "address_two":"",
                  "city":"SPOKANE",
                  "state":"WA",
                  "zip":"99210",
                  "fec_committee_id":"C00390476",
                  "cycle":2010,
                  "district":"05",
                  "created_at":null,
                  "updated_at":null,
                  "office_state":"WA",
                  "cand_status":"C",
                  "branch":"H",
                  "current":null
               },
               "relative_uri":"/candidates/H4WA05077.json"
            },
            {
               "cycle":{
                  "id":12694,
                  "fecid":"H4WA05077",
                  "name":"RODGERS, CATHY MCMORRIS",
                  "party":"REP",
                  "party3":null,
                  "seat":null,
                  "status":"I",
                  "address_one":"PO BOX 137",
                  "address_two":"",
                  "city":"SPOKANE",
                  "state":"WA",
                  "zip":"99210",
                  "fec_committee_id":"C00390476",
                  "cycle":2008,
                  "district":"05",
                  "created_at":null,
                  "updated_at":null,
                  "office_state":"WA",
                  "cand_status":"C",
                  "branch":"H",
                  "current":null
               },
               "relative_uri":"/candidates/H4WA05077.json"
            },
            {
               "cycle":{
                  "id":15644,
                  "fecid":"H4WA05077",
                  "name":"RODGERS, CATHY MCMORRIS",
                  "party":"REP",
                  "party3":null,
                  "seat":null,
                  "status":"I",
                  "address_one":"PO BOX 137",
                  "address_two":"",
                  "city":"SPOKANE",
                  "state":"WA",
                  "zip":"99210",
                  "fec_committee_id":"C00390476",
                  "cycle":2006,
                  "district":"05",
                  "created_at":null,
                  "updated_at":null,
                  "office_state":"WA",
                  "cand_status":"P",
                  "branch":"H",
                  "current":null
               },
               "relative_uri":"/candidates/H4WA05077.json"
            },
            {
               "cycle":{
                  "id":18773,
                  "fecid":"H4WA05077",
                  "name":"MCMORRIS, CATHY",
                  "party":"REP",
                  "party3":null,
                  "seat":null,
                  "status":"O",
                  "address_one":"PO BOX 555",
                  "address_two":"",
                  "city":"COLVILLE",
                  "state":"WA",
                  "zip":"99114",
                  "fec_committee_id":"C00390476",
                  "cycle":2004,
                  "district":"05",
                  "created_at":null,
                  "updated_at":null,
                  "office_state":"WA",
                  "cand_status":"C",
                  "branch":"H",
                  "current":null
               },
               "relative_uri":"/candidates/H4WA05077.json"
            }
         ]
      }
   ]
}
```

This endpoint retrieves a specific FEC candidate.


### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/candidates/{fec-id}`

### URL Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a candidate. To find a candidate's official FEC ID, use a candidate search request or the [FEC web site](http://www.fec.gov).
