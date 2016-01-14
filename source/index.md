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
require 'campaign_cash'

CampaignCash::Base.api_key = YOUR_API_KEY
CampaignCash::Candidate.search("Carson", 2016)
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: YOUR_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 Pro Publica Inc. All Rights Reserved.",
   "cycle":2016,
   "base_uri":"http://api.propublica.org/svc/elections/us/v3/finances/2016/",
   "num_results":2,
   "offset":null,
   "results":[
      {
         "candidate":{
            "id":"H8IN07184",
            "relative_uri":"/candidates/H8IN07184.json",
            "name":"CARSON, ANDRE",
            "party":"DEM"
         },
         "committee":"/committees/C00442921.json",
         "state":"/seats/IN.json",
         "district":"/seats/IN/house/07.json"
      },
      {
         "candidate":{
            "id":"P60005915",
            "relative_uri":"/candidates/P60005915.json",
            "name":"CARSON, BENJAMIN S SR MD",
            "party":"REP"
         },
         "committee":"/committees/C00573519.json",
         "state":null,
         "district":null
      }
   ]
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
   "base_uri":"http://localhost:3000/svc/elections/us/v3/finances/2016/",
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
            2014,
            2012,
            2010,
            2008,
            2006,
            2004
         ]
      }
   ]
}
```

This endpoint retrieves a specific FEC candidate for a given campaign cycle.

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/candidates/{fec-id}`

### URL Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a candidate. To find a candidate's official FEC ID, use a candidate search request or the [FEC web site](http://www.fec.gov).


## Get Top 20 Candidates in Specific Financial Category

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
   "cycle":2016,
   "category":"Contributions from PACs",
   "base_uri":"http://api.propublica.org/svc/elections/us/v3/finances/2016/",
   "results":[
      {
         "relative_uri":"/candidates/S4NC00089.json",
         "name":"BURR, RICHARD",
         "party":"REP",
         "state":"/seats/NC.json",
         "district":"00",
         "committee":"/committees/C00385526.json",
         "status":"I",
         "total_from_individuals":2106573.0,
         "total_from_pacs":1590450.0,
         "total_contributions":3697023.0,
         "candidate_loans":null,
         "total_disbursements":831188.0,
         "begin_cash":747123.0,
         "end_cash":4746624.0,
         "total_refunds":2900.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S8NY00082.json",
         "name":"SCHUMER, CHARLES E",
         "party":"DEM",
         "state":"/seats/NY.json",
         "district":"00",
         "committee":"/committees/C00346312.json",
         "status":"I",
         "total_from_individuals":8003845.0,
         "total_from_pacs":1584169.0,
         "total_contributions":9588015.0,
         "candidate_loans":null,
         "total_disbursements":1126650.0,
         "begin_cash":13409983.0,
         "end_cash":22074262.0,
         "total_refunds":136850.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S0OH00133.json",
         "name":"PORTMAN, ROB",
         "party":"REP",
         "state":"/seats/OH.json",
         "district":"00",
         "committee":"/committees/C00458463.json",
         "status":"I",
         "total_from_individuals":5704427.28,
         "total_from_pacs":1559273.5,
         "total_contributions":7263700.78,
         "candidate_loans":null,
         "total_disbursements":2439342.41,
         "begin_cash":5808793.79,
         "end_cash":11100663.0,
         "total_refunds":89574.23,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/H6CA22125.json",
         "name":"MCCARTHY, KEVIN",
         "party":"REP",
         "state":"/seats/CA.json",
         "district":"23",
         "committee":"/committees/C00420935.json",
         "status":"I",
         "total_from_individuals":867377.9,
         "total_from_pacs":1539374.02,
         "total_contributions":2406751.92,
         "candidate_loans":null,
         "total_disbursements":1696049.2,
         "begin_cash":1854633.07,
         "end_cash":4014161.84,
         "total_refunds":13100.0,
         "debts_owed":10000.0,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S4PA00121.json",
         "name":"TOOMEY, PATRICK JOSEPH",
         "party":"REP",
         "state":"/seats/PA.json",
         "district":"00",
         "committee":"/committees/C00461046.json",
         "status":"I",
         "total_from_individuals":4447328.0,
         "total_from_pacs":1459190.0,
         "total_contributions":5908519.0,
         "candidate_loans":null,
         "total_disbursements":3489592.0,
         "begin_cash":5824553.0,
         "end_cash":8598021.0,
         "total_refunds":118985.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S0MO00183.json",
         "name":"BLUNT, ROY",
         "party":"REP",
         "state":"/seats/MO.json",
         "district":"00",
         "committee":"/committees/C00304758.json",
         "status":"I",
         "total_from_individuals":2122431.0,
         "total_from_pacs":1436985.0,
         "total_contributions":3606216.0,
         "candidate_loans":null,
         "total_disbursements":1528334.0,
         "begin_cash":2213935.0,
         "end_cash":4371781.0,
         "total_refunds":48853.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S6GA00119.json",
         "name":"ISAKSON, JOHN HARDY",
         "party":"REP",
         "state":"/seats/GA.json",
         "district":"00",
         "committee":"/committees/C00384693.json",
         "status":"I",
         "total_from_individuals":2648053.0,
         "total_from_pacs":1319052.0,
         "total_contributions":3968106.0,
         "candidate_loans":null,
         "total_disbursements":1107182.0,
         "begin_cash":2423462.0,
         "end_cash":5388576.0,
         "total_refunds":51950.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S0CO00211.json",
         "name":"BENNET, MICHAEL F",
         "party":"DEM",
         "state":"/seats/CO.json",
         "district":"00",
         "committee":"/committees/C00458398.json",
         "status":"I",
         "total_from_individuals":4245124.0,
         "total_from_pacs":1271587.0,
         "total_contributions":5525238.0,
         "candidate_loans":null,
         "total_disbursements":1546322.0,
         "begin_cash":1234402.0,
         "end_cash":5387554.0,
         "total_refunds":25256.0,
         "debts_owed":374819.0,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S2WA00189.json",
         "name":"MURRAY, PATTY",
         "party":"DEM",
         "state":"/seats/WA.json",
         "district":"00",
         "committee":"/committees/C00257642.json",
         "status":"I",
         "total_from_individuals":2683897.0,
         "total_from_pacs":1212751.0,
         "total_contributions":3896649.0,
         "candidate_loans":null,
         "total_disbursements":1104183.0,
         "begin_cash":1914038.0,
         "end_cash":4731231.0,
         "total_refunds":27263.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S4SC00240.json",
         "name":"SCOTT, TIMOTHY E",
         "party":"REP",
         "state":"/seats/SC.json",
         "district":"00",
         "committee":"/committees/C00540302.json",
         "status":"I",
         "total_from_individuals":954473.0,
         "total_from_pacs":1203533.0,
         "total_contributions":2700194.0,
         "candidate_loans":null,
         "total_disbursements":1007553.0,
         "begin_cash":2480147.0,
         "end_cash":4235827.0,
         "total_refunds":17701.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S2SD00068.json",
         "name":"THUNE, JOHN",
         "party":"REP",
         "state":"/seats/SD.json",
         "district":"00",
         "committee":"/committees/C00409581.json",
         "status":"I",
         "total_from_individuals":981548.0,
         "total_from_pacs":1089198.0,
         "total_contributions":2070746.0,
         "candidate_loans":null,
         "total_disbursements":797059.0,
         "begin_cash":9715362.0,
         "end_cash":11208816.0,
         "total_refunds":13315.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S4AK00099.json",
         "name":"MURKOWSKI, LISA",
         "party":"REP",
         "state":"/seats/AK.json",
         "district":"00",
         "committee":"/committees/C00384529.json",
         "status":"I",
         "total_from_individuals":1474597.0,
         "total_from_pacs":1008847.0,
         "total_contributions":2483444.0,
         "candidate_loans":null,
         "total_disbursements":697596.0,
         "begin_cash":903629.0,
         "end_cash":2911178.0,
         "total_refunds":29200.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/H8WI01024.json",
         "name":"RYAN, PAUL D.",
         "party":"REP",
         "state":"/seats/WI.json",
         "district":"01",
         "committee":"/committees/C00330894.json",
         "status":"I",
         "total_from_individuals":1527228.51,
         "total_from_pacs":992975.0,
         "total_contributions":2520203.51,
         "candidate_loans":null,
         "total_disbursements":888054.68,
         "begin_cash":2680320.86,
         "end_cash":4546894.27,
         "total_refunds":2525.0,
         "debts_owed":33525.51,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/H0OH08029.json",
         "name":"BOEHNER, JOHN A.",
         "party":"REP",
         "state":"/seats/OH.json",
         "district":"08",
         "committee":"/committees/C00237198.json",
         "status":"I",
         "total_from_individuals":790479.84,
         "total_from_pacs":985600.0,
         "total_contributions":1776079.84,
         "candidate_loans":null,
         "total_disbursements":2448788.72,
         "begin_cash":1293215.88,
         "end_cash":3787340.8,
         "total_refunds":2200.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-06-30"
      },
      {
         "relative_uri":"/candidates/S0IL00261.json",
         "name":"KIRK, MARK STEVEN",
         "party":"REP",
         "state":"/seats/IL.json",
         "district":"00",
         "committee":"/committees/C00350785.json",
         "status":"I",
         "total_from_individuals":2293804.0,
         "total_from_pacs":982713.0,
         "total_contributions":3287517.0,
         "candidate_loans":null,
         "total_disbursements":1863167.0,
         "begin_cash":2005145.0,
         "end_cash":3627428.0,
         "total_refunds":56525.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S6FL00319.json",
         "name":"MURPHY, PATRICK E",
         "party":"DEM",
         "state":"/seats/FL.json",
         "district":"00",
         "committee":"/committees/C00493825.json",
         "status":"O",
         "total_from_individuals":3205400.0,
         "total_from_pacs":974450.0,
         "total_contributions":4180816.0,
         "candidate_loans":null,
         "total_disbursements":1265171.0,
         "begin_cash":536659.0,
         "end_cash":3476853.0,
         "total_refunds":8812.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S0IA00028.json",
         "name":"GRASSLEY, CHARLES E SENATOR",
         "party":"REP",
         "state":"/seats/IA.json",
         "district":"00",
         "committee":"/committees/C00230482.json",
         "status":"I",
         "total_from_individuals":1573289.0,
         "total_from_pacs":947190.0,
         "total_contributions":2520479.0,
         "candidate_loans":null,
         "total_disbursements":572319.0,
         "begin_cash":1838647.0,
         "end_cash":3928181.0,
         "total_refunds":9000.0,
         "debts_owed":11257.0,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/H2PA09035.json",
         "name":"SHUSTER, WILLIAM MR.",
         "party":"REP",
         "state":"/seats/PA.json",
         "district":"09",
         "committee":"/committees/C00364935.json",
         "status":"I",
         "total_from_individuals":551613.64,
         "total_from_pacs":939850.0,
         "total_contributions":1491713.64,
         "candidate_loans":null,
         "total_disbursements":604956.37,
         "begin_cash":415297.62,
         "end_cash":1312343.09,
         "total_refunds":1000.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S0KS00091.json",
         "name":"MORAN, JERRY",
         "party":"REP",
         "state":"/seats/KS.json",
         "district":"00",
         "committee":"/committees/C00458315.json",
         "status":"I",
         "total_from_individuals":1065732.0,
         "total_from_pacs":933881.0,
         "total_contributions":1999614.0,
         "candidate_loans":null,
         "total_disbursements":754453.0,
         "begin_cash":1380214.0,
         "end_cash":2687687.0,
         "total_refunds":20600.0,
         "debts_owed":127377.0,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      },
      {
         "relative_uri":"/candidates/S8ID00027.json",
         "name":"CRAPO, MICHAEL D",
         "party":"REP",
         "state":"/seats/ID.json",
         "district":"00",
         "committee":"/committees/C00330886.json",
         "status":"I",
         "total_from_individuals":791973.0,
         "total_from_pacs":923304.0,
         "total_contributions":1716277.0,
         "candidate_loans":null,
         "total_disbursements":772109.0,
         "begin_cash":3500640.0,
         "end_cash":4497386.0,
         "total_refunds":4705.0,
         "debts_owed":null,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-09-30"
      }
   ]
}
```

This endpoint retrieves a specific FEC candidate for a given campaign cycle.

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/candidates/leaders/{category}`

### URL Parameters

Parameter | Description
--------- | -----------
category | One of the values from the following categories:

             Category | Value
             ----- | -----
             Candidate Loan | `candidate-loan`
             Contribution Total | `contribution-total`
             Debts Owed | `debts-owed`
             Disbursements Total | `disbursements-total`
             End Cash | `end-cash`
             Individual Total | `individual-total`
             PAC Total | `pac-total`
             Receipts Total | `receipts-total`
             Refund Total | `refund-total`

## Get Candidates from a State

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
   "cycle":2016,
   "base_uri":"http://api.propublica.org/svc/elections/us/v3/finances/2016/",
   "num_results":2,
   "results":[
      {
         "candidate":{
            "id":"H0DE01017",
            "relative_uri":"/candidates/H0DE01017.json",
            "name":"CARNEY, JOHN",
            "party":"DEM"
         },
         "committee":"/committees/C00460899.json",
         "state":"/seats/DE.json",
         "district":null
      },
      {
         "candidate":{
            "id":"H6DE01071",
            "relative_uri":"/candidates/H6DE01071.json",
            "name":"REIGLE, HANS",
            "party":"REP"
         },
         "committee":"/committees/C00574129.json",
         "state":"/seats/DE.json",
         "district":null
      }
   ]
}
```

This endpoint retrieves an array of FEC candidates for a given state (and optional chamber and district).

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/candidates/states/{state}/{chamber}/{district}`

### URL Parameters

Parameter | Description
--------- | -----------
state | Two-letter state abbreviation
chamber | `house` or `senate` (optional)
district | Specify the district number. Use `1` for states with a single representative. (House requests only - districts with Senate requests will be ignored.)


## Get Recently Added Candidates

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
   "cycle":2016,
   "base_uri":"http://api.propublica.org/svc/elections/us/v3/finances/2016/",
   "results":[
      {
         "id":"P60005162",
         "relative_uri":"/candidates/P60005162.json",
         "name":"DEBOW, MR PAUL W",
         "district":null,
         "mailing_city":"NATCHITOCHOS",
         "mailing_state":"LA",
         "mailing_zip":"71547",
         "party":"REP",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60005162",
         "committee":"/committees/C00562512.json"
      },
      {
         "id":"H6MN06157",
         "relative_uri":"/candidates/H6MN06157.json",
         "name":"HELLAND, ROBERT RANDOLPH",
         "district":"/seats/MN/house/06.json",
         "mailing_city":"ST PAUL ",
         "mailing_state":"MN",
         "mailing_zip":"55105",
         "party":"UNK",
         "state":"/seats/MN.json",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?H6MN06157",
         "committee":"/committees/C00587907.json"
      },
      {
         "id":"P60004918",
         "relative_uri":"/candidates/P60004918.json",
         "name":"TAPE , PAUL EDWARD JR",
         "district":null,
         "mailing_city":"TRENTON",
         "mailing_state":"FL",
         "mailing_zip":"32693",
         "party":"OTH",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60004918",
         "committee":"/committees/C00547521.json"
      },
      {
         "id":"P60006970",
         "relative_uri":"/candidates/P60006970.json",
         "name":"DELL, CRAIG STEVEN",
         "district":null,
         "mailing_city":"NORFOLK",
         "mailing_state":"VA",
         "mailing_zip":"23503",
         "party":"IND",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60006970",
         "committee":null
      },
      {
         "id":"P60006988",
         "relative_uri":"/candidates/P60006988.json",
         "name":"WALSH, JOHN R III",
         "district":null,
         "mailing_city":"SCRANTON",
         "mailing_state":"PA",
         "mailing_zip":"18501",
         "party":"DEM",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60006988",
         "committee":null
      },
      {
         "id":"S0NH00235",
         "relative_uri":"/candidates/S0NH00235.json",
         "name":"AYOTTE, KELLY A",
         "district":"/seats/NH/senate.json",
         "mailing_city":"MANCHESTER",
         "mailing_state":"NH",
         "mailing_zip":"03105",
         "party":"REP",
         "state":"/seats/NH.json",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?S0NH00235",
         "committee":"/committees/C00464297.json"
      },
      {
         "id":"H0WI08075",
         "relative_uri":"/candidates/H0WI08075.json",
         "name":"RIBBLE, REID J. REP.",
         "district":"/seats/WI/house/08.json",
         "mailing_city":"SHERWOOD",
         "mailing_state":"WI",
         "mailing_zip":"54169",
         "party":"REP",
         "state":"/seats/WI.json",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?H0WI08075",
         "committee":"/committees/C00463620.json"
      },
      {
         "id":"S6CO00242",
         "relative_uri":"/candidates/S6CO00242.json",
         "name":"KINLAW, MICHAEL",
         "district":"/seats/CO/senate.json",
         "mailing_city":"COLORADO SPRINGS",
         "mailing_state":"CO",
         "mailing_zip":"80949",
         "party":"REP",
         "state":"/seats/CO.json",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?S6CO00242",
         "committee":"/committees/C00576298.json"
      },
      {
         "id":"P60012747",
         "relative_uri":"/candidates/P60012747.json",
         "name":"PARKS, DANIEL KITTREDGE",
         "district":null,
         "mailing_city":"MIMBRES",
         "mailing_state":"IA",
         "mailing_zip":"88049",
         "party":"PAF",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60012747",
         "committee":null
      },
      {
         "id":"P60015922",
         "relative_uri":"/candidates/P60015922.json",
         "name":"LEE, THOMAS PAUL",
         "district":null,
         "mailing_city":"GREENSBORO",
         "mailing_state":"NC",
         "mailing_zip":"27401",
         "party":"IND",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60015922",
         "committee":"/committees/C00587915.json"
      },
      {
         "id":"P60015930",
         "relative_uri":"/candidates/P60015930.json",
         "name":"SWIFT, TAYLOR",
         "district":null,
         "mailing_city":"WASHINGTON",
         "mailing_state":"DC",
         "mailing_zip":"20003",
         "party":"UNK",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60015930",
         "committee":null
      },
      {
         "id":"P60015948",
         "relative_uri":"/candidates/P60015948.json",
         "name":"WILLIAMS, SAUL III",
         "district":null,
         "mailing_city":"LAS VEGAS",
         "mailing_state":"NV",
         "mailing_zip":"89119",
         "party":"DEM",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60015948",
         "committee":null
      },
      {
         "id":"P60015955",
         "relative_uri":"/candidates/P60015955.json",
         "name":"DEWEY, JARED ALEXANDER",
         "district":null,
         "mailing_city":"MESA ",
         "mailing_state":"AZ",
         "mailing_zip":"85202",
         "party":"REP",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60015955",
         "committee":null
      },
      {
         "id":"P60015989",
         "relative_uri":"/candidates/P60015989.json",
         "name":"KUSH, GEORGE W MLG",
         "district":null,
         "mailing_city":"WEED",
         "mailing_state":"CA",
         "mailing_zip":"96404",
         "party":"IND",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60015989",
         "committee":null
      },
      {
         "id":"P60015997",
         "relative_uri":"/candidates/P60015997.json",
         "name":"SERINO, CHRISTIAN THOMAS",
         "district":null,
         "mailing_city":"HENDERSON",
         "mailing_state":"NV",
         "mailing_zip":"89012",
         "party":"DEM",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60015997",
         "committee":"/committees/C00587956.json"
      },
      {
         "id":"P60016003",
         "relative_uri":"/candidates/P60016003.json",
         "name":"WESTRTHOSE, KANYEWHAT DEEZNUTZ KANYE T",
         "district":null,
         "mailing_city":"CHINO HILLS",
         "mailing_state":"CA",
         "mailing_zip":"91709",
         "party":"IND",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60016003",
         "committee":null
      },
      {
         "id":"P60016011",
         "relative_uri":"/candidates/P60016011.json",
         "name":"SUPRUN, DANIEL FRANCIS MR",
         "district":null,
         "mailing_city":"BRISTOL",
         "mailing_state":"CT",
         "mailing_zip":"06010",
         "party":"IND",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P60016011",
         "committee":null
      },
      {
         "id":"P80003205",
         "relative_uri":"/candidates/P80003205.json",
         "name":"MERCER, LEE L JR",
         "district":null,
         "mailing_city":"HOUSTON",
         "mailing_state":"TX",
         "mailing_zip":"77021",
         "party":"DEM",
         "state":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?P80003205",
         "committee":"/committees/C00428599.json"
      },
      {
         "id":"H4ME02200",
         "relative_uri":"/candidates/H4ME02200.json",
         "name":"CAIN, EMILY",
         "district":"/seats/ME/house/02.json",
         "mailing_city":"BANGOR",
         "mailing_state":"ME",
         "mailing_zip":"04402",
         "party":"DEM",
         "state":"/seats/ME.json",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?H4ME02200",
         "committee":"/committees/C00546077.json"
      },
      {
         "id":"H6AL02142",
         "relative_uri":"/candidates/H6AL02142.json",
         "name":"GERRITSON, REBECCA (BECKY)",
         "district":"/seats/AL/house/02.json",
         "mailing_city":"WETUMPKA",
         "mailing_state":"AL",
         "mailing_zip":"36902",
         "party":"REP",
         "state":"/seats/AL.json",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?H6AL02142",
         "committee":"/committees/C00588244.json"
      }
   ]
}
```

This endpoint retrieves the 20 most recently added FEC candidates.

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/candidates/new`

# Committees

## Search for Committees

```ruby
require 'campaign_cash'

CampaignCash::Base.api_key = YOUR_API_KEY
CampaignCash::Committee.search("Carson", 2016)
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: YOUR_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "cycle":2016,
   "base_uri":"http://api.propublica.org/svc/elections/us/v3/finances/2016",
   "num_results":8,
   "offset":null,
   "results":[
      {
         "id":"C00570788",
         "relative_uri":"/committees/C00570788.json",
         "name":"AMERICANS FOR A BETTER TOMORROW TODAY",
         "party":"",
         "treasurer":"EVAN WICK",
         "address":"FLC 7788 1000 RIM DRIVE",
         "leadership":false,
         "city":"DURANGO",
         "state":"CO",
         "zip":"81301",
         "candidate":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00570788/"
      },
      {
         "id":"C00560243",
         "relative_uri":"/committees/C00560243.json",
         "name":"AMERICANS FOR A BETTER FUTURE",
         "party":"",
         "treasurer":"NICK MANIS",
         "address":"11 DRINKING BROOK RD",
         "leadership":false,
         "city":"MONMOUTH JCT",
         "state":"NJ",
         "zip":"08852",
         "candidate":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00560243/"
      },
      {
         "id":"C00404566",
         "relative_uri":"/committees/C00404566.json",
         "name":"AMERICANS FOR A BETTER WAY",
         "party":"",
         "treasurer":"ERLENBORN, ERIN",
         "address":"213 F STREET NE",
         "leadership":false,
         "city":"WASHINGTON",
         "state":"DC",
         "zip":"20002",
         "candidate":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00404566/"
      },
      {
         "id":"C00533125",
         "relative_uri":"/committees/C00533125.json",
         "name":"AMERICANS FOR A BETTER TOMORROW PERIOD",
         "party":"",
         "treasurer":"DONAHUE, JOSEPH GERARD",
         "address":"701 CITY AVENUE #A-407",
         "leadership":false,
         "city":"MERION STATION",
         "state":"PA",
         "zip":"19066",
         "candidate":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00533125/"
      },
      {
         "id":"C00567255",
         "relative_uri":"/committees/C00567255.json",
         "name":"AMERICANS FOR A BETTER YESTERDAY, TOMORROW",
         "party":"",
         "treasurer":"WILLIAM MCCABE CZERWINSKI",
         "address":"27475 BLUE RIDGE LANE",
         "leadership":false,
         "city":"EXCELSIOR",
         "state":"MN",
         "zip":"55331",
         "candidate":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00567255/"
      },
      {
         "id":"C00567776",
         "relative_uri":"/committees/C00567776.json",
         "name":"MIDDLE AMERICANS FOR A BETTER TOMORROW",
         "party":"",
         "treasurer":"LISA WERDERMAN",
         "address":"1602 2ND AVE",
         "leadership":false,
         "city":"SAN MATEO",
         "state":"CA",
         "zip":"94401",
         "candidate":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00567776/"
      },
      {
         "id":"C00567784",
         "relative_uri":"/committees/C00567784.json",
         "name":"BLACK AMERICANS FOR A BETTER FUTURE",
         "party":"",
         "treasurer":"MARSTON, CHRIS",
         "address":"45 N HILL DR",
         "leadership":false,
         "city":"WARRENTON",
         "state":"VA",
         "zip":"20186",
         "candidate":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00567784/"
      },
      {
         "id":"C00570481",
         "relative_uri":"/committees/C00570481.json",
         "name":"AMERICANS FOR A BETTER YESTERDAY",
         "party":"",
         "treasurer":"BRANDON SCOTT ALLEN CLARK",
         "address":"9813 TOBIN CIR",
         "leadership":false,
         "city":"NORMAN",
         "state":"OK",
         "zip":"73026",
         "candidate":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00570481/"
      }
   ]
}
```

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/committees/search`

### Query Parameters

Parameter | Description
--------- | -----------
query | The name of the committee


## Get a Specific Committee

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
   "cycle":2016,
   "base_uri":"http://api.propublica.org/svc/elections/us/v3/finances/2016/",
   "results":[
      {
         "id":"C00553560",
         "name":"VIGOP (VIRGIN ISLANDS REPUBLICAN PARTY)",
         "party":"REP",
         "treasurer":"MACKENZIE, SCOTT B",
         "address":"PO BOX 295",
         "city":"CHRISTIANSTED",
         "state":"VI",
         "zip":"00821",
         "total_receipts":1210554.8,
         "total_from_individuals":1206010.8,
         "total_from_pacs":190.0,
         "total_contributions":1206200.8,
         "total_disbursements":1187888.77,
         "begin_cash":45482.65,
         "end_cash":68148.68,
         "total_refunds":0.0,
         "display_type":"PAC",
         "debts_owed":226265.94,
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-11-30",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/",
         "total_independent_expenditures":0.0,
         "candidate":null,
         "leadership":false,
         "super_pac":false,
         "total_candidate_contributions":2500.0,
         "transfers_in":0.0,
         "designation":"U",
         "filing_frequency":"M",
         "committee_type":"Q",
         "interest_group":null,
         "total_coordinated_expenditures":null,
         "end_date":"2015-11-30",
         "next_filing_date":"2016-01-31",
         "other_cycles":[2014]
      }
   ]
}
```

This endpoint retrieves a specific FEC committee for a given campaign cycle.

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/committees/{fec-id}`

### URL Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a committee. To find a committee's official FEC ID, use a committee search request or the [FEC web site](http://www.fec.gov).

## Get Recently Added Committees

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
```

This endpoint retrieves the 20 most recently added FEC committees.

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/committees/new`

## Get Committee Filings

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
   "base_uri":"http://api.propublica.org/svc/elections/us/v3/finances/2016/",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "committee":"/committees/C00553560.json",
   "fec_committee_id":"C00553560",
   "offset":null,
   "results":[
      {
         "id":1035569,
         "cycle":2016,
         "form_type":"F3",
         "date_filed":"2015-12-13",
         "date_coverage_to":"2015-11-30",
         "date_coverage_from":"2015-11-01",
         "report_title":"DEC MONTHLY",
         "report_period":"M12",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/1035569/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":155272.7,
         "cash_on_hand":68148.68,
         "disbursements_total":113494.27,
         "receipts_total":157043.7
      },
      {
         "id":1032959,
         "cycle":2016,
         "form_type":"F3",
         "date_filed":"2015-11-16",
         "date_coverage_to":"2015-10-31",
         "date_coverage_from":"2015-10-01",
         "report_title":"NOV MONTHLY",
         "report_period":"M11",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/1032959/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":173500.79,
         "cash_on_hand":24599.25,
         "disbursements_total":196765.57,
         "receipts_total":173584.79
      },
      {
         "id":1030722,
         "cycle":2016,
         "form_type":"F3",
         "date_filed":"2015-10-20",
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-09-01",
         "report_title":"OCT MONTHLY",
         "report_period":"M10",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/1030722/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":96139.78,
         "cash_on_hand":47780.03,
         "disbursements_total":101543.31,
         "receipts_total":96181.78
      },
      {
         "id":1025866,
         "cycle":2016,
         "form_type":"F3",
         "date_filed":"2015-09-20",
         "date_coverage_to":"2015-08-31",
         "date_coverage_from":"2015-08-01",
         "report_title":"SEP MONTHLY",
         "report_period":"M9",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/1025866/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":152725.15,
         "cash_on_hand":53141.56,
         "disbursements_total":107782.84,
         "receipts_total":152738.15
      },
      {
         "id":1016418,
         "cycle":2016,
         "form_type":"F3",
         "date_filed":"2015-07-18",
         "date_coverage_to":"2015-06-30",
         "date_coverage_from":"2015-06-01",
         "report_title":"JUL MONTHLY",
         "report_period":"M7",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/1016418/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":86970.46,
         "cash_on_hand":8959.51,
         "disbursements_total":96117.86,
         "receipts_total":86997.46
      },
      {
         "id":1011437,
         "cycle":2016,
         "form_type":"F3",
         "date_filed":"2015-06-20",
         "date_coverage_to":"2015-05-31",
         "date_coverage_from":"2015-05-01",
         "report_title":"JUN MONTHLY",
         "report_period":"M6",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/1011437/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":113864.19,
         "cash_on_hand":18079.91,
         "disbursements_total":126883.76,
         "receipts_total":113935.19
      },
      {
         "id":1007741,
         "cycle":2016,
         "form_type":"F1M",
         "date_filed":"2015-05-18",
         "date_coverage_to":null,
         "date_coverage_from":null,
         "report_title":"NOTIFICATION OF MULTICANDIDATE STATUS",
         "report_period":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/1007741/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null
      },
      {
         "id":1007739,
         "cycle":2016,
         "form_type":"F3",
         "date_filed":"2015-05-18",
         "date_coverage_to":"2015-04-30",
         "date_coverage_from":"2015-04-01",
         "report_title":"MAY MONTHLY",
         "report_period":"M5",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/1007739/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":154150.89,
         "cash_on_hand":31028.48,
         "disbursements_total":148489.36,
         "receipts_total":155222.89
      },
      {
         "id":1005430,
         "cycle":2016,
         "form_type":"F3",
         "date_filed":"2015-04-20",
         "date_coverage_to":"2015-03-31",
         "date_coverage_from":"2015-03-01",
         "report_title":"APR MONTHLY",
         "report_period":"M4",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/1005430/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":62721.86,
         "cash_on_hand":24294.95,
         "disbursements_total":72980.51,
         "receipts_total":63179.86
      },
      {
         "id":999014,
         "cycle":2016,
         "form_type":"F3",
         "date_filed":"2015-03-20",
         "date_coverage_to":"2015-02-28",
         "date_coverage_from":"2015-02-01",
         "report_title":"MAR MONTHLY",
         "report_period":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/999014/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":72137.94,
         "cash_on_hand":34095.6,
         "disbursements_total":55743.77,
         "receipts_total":72953.94
      },
      {
         "id":995134,
         "cycle":2016,
         "form_type":"F3",
         "date_filed":"2015-02-20",
         "date_coverage_to":"2015-01-31",
         "date_coverage_from":"2015-01-01",
         "report_title":"FEB MONTHLY",
         "report_period":"M2",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/995134/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":56024.79,
         "cash_on_hand":16885.43,
         "disbursements_total":84622.01,
         "receipts_total":56024.79
      },
      {
         "id":993773,
         "cycle":2016,
         "form_type":"F1",
         "date_filed":"2015-02-17",
         "date_coverage_to":null,
         "date_coverage_from":null,
         "report_title":"STATEMENT OF ORGANIZATION",
         "report_period":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/993773/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null
      },
      {
         "id":991995,
         "cycle":2016,
         "form_type":"F1",
         "date_filed":"2015-02-01",
         "date_coverage_to":null,
         "date_coverage_from":null,
         "report_title":"STATEMENT OF ORGANIZATION",
         "report_period":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/991995/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null
      },
      {
         "id":988038,
         "cycle":2016,
         "form_type":"F3",
         "date_filed":"2015-01-28",
         "date_coverage_to":"2014-12-31",
         "date_coverage_from":"2014-11-25",
         "report_title":"YEAR-END",
         "report_period":"YE",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553560/988038/",
         "paper":false,
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":89894.16,
         "cash_on_hand":45482.65,
         "disbursements_total":83431.31,
         "receipts_total":89894.16
      }
   ]
}
```

This endpoint retrieves the 20 most recently added FEC committees. Electronic filings are available back to 2001. Paper filings by Senate candidate committees and senatorial party committees go back to 1999.

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/committees/{fec-id}/filings`

### URL Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a committee. To find a committee's official FEC ID, use a committee search request or the [FEC web site](http://www.fec.gov).

## Get Recently Added Committees

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
```

This endpoint retrieves the 20 most recently added FEC committees.

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/committees/new`

## Get Leadership Committees

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
   "cycle":2016,
   "base_uri":"http://api.propublica.org/svc/elections/us/v3/finances/2016/",
   "offset":null,
   "results":[
      {
         "id":"C00602789",
         "relative_uri":"/committees/C00602789.json",
         "name":"POLITICS AND THE DEAF",
         "address":"2901 MACARTHUR BLVD",
         "city":"OAKLAND",
         "state":"CA",
         "zip":"94602",
         "treasurer":"PATTERSON, MELVIN L",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00602789/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"MELVIN   L  PATTERSON",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":""
      },
      {
         "id":"C00540187",
         "relative_uri":"/committees/C00540187.json",
         "name":"MORE CONSERVATIVES PAC (MCPAC)",
         "address":"228 S WASHINGTON ST STE 115",
         "city":"ALEXANDRIA",
         "state":"VA",
         "zip":"22314",
         "treasurer":"LISKER, LISA",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00540187/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"MCHENRY, PATRICK TIMOTHY",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"Q",
         "interest_group":""
      },
      {
         "id":"C00502591",
         "relative_uri":"/committees/C00502591.json",
         "name":"MARKETPLACE IDEAS AND CONSERVATIVE KNOWLEDGE PAC-MICK PAC",
         "address":"228 S WASHINGTON ST STE 115",
         "city":"ALEXANDRIA",
         "state":"VA",
         "zip":"22314",
         "treasurer":"LISA LISKER",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00502591/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"JOHN MICHAEL 'MICK' MULVANEY",
         "designation":"D",
         "filing_frequency":"M",
         "committee_type":"N",
         "interest_group":null
      },
      {
         "id":"C00502096",
         "relative_uri":"/committees/C00502096.json",
         "name":"MICHIGAN'S FUTURE PAC",
         "address":"PO BOX 402",
         "city":"FLINT",
         "state":"MI",
         "zip":"48501",
         "treasurer":"HOLTZ, DAVID",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00502096/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"DAN KILDEE, DANEIL",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"Q",
         "interest_group":""
      },
      {
         "id":"C00599092",
         "relative_uri":"/committees/C00599092.json",
         "name":"WOLF PACK",
         "address":"11 DUPONT CIRCLE",
         "city":"WASHINGTON",
         "state":"MD",
         "zip":"20036",
         "treasurer":"ALEXANDER, JIM",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00599092/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"EMANUEL  CLEAVER  II",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":""
      },
      {
         "id":"C00571521",
         "relative_uri":"/committees/C00571521.json",
         "name":"FREEDOM 21: FIGHTING FOR FREEDOM IN THE 21ST CENTURY",
         "address":"PO BOX 657",
         "city":"LEHI",
         "state":"UT",
         "zip":"84043",
         "treasurer":"NILSSON, JACE",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00571521/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"STEWART, CHRIS",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":""
      },
      {
         "id":"C00553271",
         "relative_uri":"/committees/C00553271.json",
         "name":"AMERICAN LEADERSHIP NOW (ALN PAC)",
         "address":"ATTN: VINCENT DEVITO ESQ BOWDITCH ",
         "city":"BOSTON",
         "state":"MA",
         "zip":"02110",
         "treasurer":"VINCENT DEVITO",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00553271/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"PETER T KING",
         "designation":"D",
         "filing_frequency":"T",
         "committee_type":"N",
         "interest_group":null
      },
      {
         "id":"C00497594",
         "relative_uri":"/committees/C00497594.json",
         "name":"DEFENDING AMERICA'S VALUES EVERYWHERE (TEAM DAVE)",
         "address":"228 S WASHINGTON STREET SUITE 115",
         "city":"ALEXANDRIA",
         "state":"VA",
         "zip":"22314",
         "treasurer":"KEITH A. DAVIS",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00497594/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"SCHWEIKERT, DAVID",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"Q",
         "interest_group":null
      },
      {
         "id":"C00464644",
         "relative_uri":"/committees/C00464644.json",
         "name":"HARRIS COUNTY CONSERVATIVE CONGRESSIONAL CAMPAIGN",
         "address":"1701 HERMANN DR #2206",
         "city":"HOUSTON",
         "state":"TX",
         "zip":"77004",
         "treasurer":"CHRIS HOLMES",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00464644/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"FAULK FOR CONGRESS",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":""
      },
      {
         "id":"C00440032",
         "relative_uri":"/committees/C00440032.json",
         "name":"PROMOTING OUR REPUBLICAN TEAM PAC",
         "address":"8331 LITTLE HARBOR DRIVE",
         "city":"CINCINNATI",
         "state":"OH",
         "zip":"45244",
         "treasurer":"BAUR, NATALIE MRS.",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00440032/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"PORTMAN VICTORY COMMITTEE",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"Q",
         "interest_group":""
      },
      {
         "id":"C00392738",
         "relative_uri":"/committees/C00392738.json",
         "name":"HOLDING ONTO OREGON'S PRIORITIES",
         "address":"PO BOX 3314",
         "city":"PORTLAND",
         "state":"OR",
         "zip":"97208",
         "treasurer":"MICHELS, F. STEPHEN",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00392738/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"WYDEN FOR OREGON",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"Q",
         "interest_group":""
      },
      {
         "id":"C00392134",
         "relative_uri":"/committees/C00392134.json",
         "name":"MAKING BUSINESS EXCEL POLITICAL ACTION COMMITTEE",
         "address":"PO BOX 2687",
         "city":"CODY",
         "state":"WY",
         "zip":"82414",
         "treasurer":"TOPE, JUNE V. MS.",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00392134/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"ENZI FOR US SENATE",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"Q",
         "interest_group":""
      },
      {
         "id":"C00597062",
         "relative_uri":"/committees/C00597062.json",
         "name":"BUDDY PAC",
         "address":"824 S  MILLEDGE AVE STE 101",
         "city":"ATHENS",
         "state":"GA",
         "zip":"30605",
         "treasurer":"KILGORE, PAUL",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00597062/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"EARL  LEROY  CARTER",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":""
      },
      {
         "id":"C00593244",
         "relative_uri":"/committees/C00593244.json",
         "name":"FRIENDS OF VALENCIA ST LOUIS LAROSE CONGRESSIONAL LEADERSHIP PAC",
         "address":"1900 WEST OAKLAND PARK BLVD.",
         "city":"FORT LAUDERDALE",
         "state":"FL",
         "zip":"33310",
         "treasurer":"ST LOUIS LAROSE, VALENCIA",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00593244/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":""
      },
      {
         "id":"C00591461",
         "relative_uri":"/committees/C00591461.json",
         "name":"NEW MEXICO WORKS PAC",
         "address":"611 PENNSYLVANIA AVE. SE",
         "city":"WASHINGTON",
         "state":"DC",
         "zip":"20003",
         "treasurer":"ARMSTRONG, DEBORAH",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00591461/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"MICHELLE  LUJAN  GRISHAM",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":""
      },
      {
         "id":"C00589309",
         "relative_uri":"/committees/C00589309.json",
         "name":"LEADERSHIP, INTEGRITY, ENGAGEMENT, UNITY PAC",
         "address":"16633 VENTURA BLVD # 1008",
         "city":"ENCINO",
         "state":"CA",
         "zip":"91436",
         "treasurer":"LEIDERMAN, JANE FED",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00589309/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"TED LIEU",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":""
      },
      {
         "id":"C00584805",
         "relative_uri":"/committees/C00584805.json",
         "name":"BFB PAC",
         "address":"499 S. CAPITOL STREET, SW",
         "city":"WASHINGTON",
         "state":"DC",
         "zip":"20003",
         "treasurer":"ANGERHOLZER, LINDSAY",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00584805/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"HON.  BRENDAN  BOYLE",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":""
      },
      {
         "id":"C00581751",
         "relative_uri":"/committees/C00581751.json",
         "name":"ASSOCIATION OF MISSOURI NURSE PRACTITIONER'S POLITICAL ACTION COMMITTEE",
         "address":"104 KINGSLEY DR",
         "city":"MONETT",
         "state":"MO",
         "zip":"65708",
         "treasurer":"JANICE JONES, DNP APRN",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00581751/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":"C"
      },
      {
         "id":"C00576975",
         "relative_uri":"/committees/C00576975.json",
         "name":"LATINO LEADERS FOR EQUALITY GROWTH OPPORTUNITY PROGRESSIVE ACTION & CHANGE (LLEGO-PAC)",
         "address":"1050 17TH ST NW STE 590",
         "city":"WASHINGTON",
         "state":"DC",
         "zip":"20036",
         "treasurer":"INGRID DURAN",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00576975/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"RUBEN GALLEGO",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":""
      },
      {
         "id":"C00573709",
         "relative_uri":"/committees/C00573709.json",
         "name":"CA LUV PAC (CALIFORNIA LEADERSHIP UNITED FOR VICTORY PAC)",
         "address":"410 1ST ST, SE",
         "city":"WASHINGTON",
         "state":"DC",
         "zip":"20003",
         "treasurer":"MAY, JENNIFER",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00573709/",
         "candidate":null,
         "leadership":true,
         "super_pac":false,
         "sponsor_name":"PETE  AGUILAR",
         "designation":"D",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":""
      }
   ]
}
```

This endpoint retrieves committees designated as "[leadership PACs](http://www.fec.gov/finance/disclosure/metadata/metadataLeadershipPacList.shtml)" by the FEC.

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/committees/leadership`

# Presidential Campaigns

## Presidential Candidate Totals

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "base_uri":"http://localhost:3000/svc/elections/us/v3/finances/2016/",
   "cycle":2016,
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "results":[
      {
         "slug":"clinton",
         "candidate_name":"Clinton, Hillary",
         "name":"Hillary Clinton",
         "party":"D",
         "committee":"committees/C00575795.json",
         "committee_id":"C00575795",
         "total_receipts":77471603.55,
         "total_disbursements":44476431.16,
         "cash_on_hand":32995172.39,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P00003392",
         "contributions_less_than_200":15721441.79,
         "contributions_200_499":2459122.17,
         "contributions_500_1499":8171798.67,
         "contributions_1500_2699":5212541.31,
         "contributions_2700":44973900.0,
         "net_primary":31468252.25,
         "net_primary_pct":40.6,
         "contributions_less_than_200_pct":20.4,
         "contributions_2700_pct":58.4,
         "burn_rate":86.14703324065016,
         "total_contributions":76995137.24
      },
      {
         "slug":"sanders",
         "candidate_name":"Sanders, Bernie",
         "name":"Bernie Sanders",
         "party":"D",
         "committee":"committees/C00577130.json",
         "committee_id":"C00577130",
         "total_receipts":41463783.81,
         "total_disbursements":14344062.31,
         "cash_on_hand":27119721.5,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P60007168",
         "contributions_less_than_200":35242701.59,
         "contributions_200_499":3415085.97,
         "contributions_500_1499":4453633.72,
         "contributions_1500_2699":465673.0,
         "contributions_2700":491400.0,
         "net_primary":27119721.5,
         "net_primary_pct":65.4,
         "contributions_less_than_200_pct":88.2,
         "contributions_2700_pct":1.2,
         "burn_rate":42.94423911574494,
         "total_contributions":39953743.73
      },
      {
         "slug":"carson",
         "candidate_name":"Carson, Ben",
         "name":"Ben Carson",
         "party":"R",
         "committee":"committees/C00573519.json",
         "committee_id":"C00573519",
         "total_receipts":31409508.61,
         "total_disbursements":20136974.48,
         "cash_on_hand":11272534.13,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P60005915",
         "contributions_less_than_200":23031428.65,
         "contributions_200_499":2437521.72,
         "contributions_500_1499":3700653.27,
         "contributions_1500_2699":609745.99,
         "contributions_2700":934200.0,
         "net_primary":11063181.41,
         "net_primary_pct":35.199999999999996,
         "contributions_less_than_200_pct":73.5,
         "contributions_2700_pct":3.0,
         "burn_rate":68.56966227665559,
         "total_contributions":31336023.41
      },
      {
         "slug":"cruz",
         "candidate_name":"Cruz, Ted",
         "name":"Ted Cruz",
         "party":"R",
         "committee":"committees/C00574624.json",
         "committee_id":"C00574624",
         "total_receipts":26567298.26,
         "total_disbursements":12788394.22,
         "cash_on_hand":13778904.04,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-01-01",
         "candidate_id":"P60006111",
         "contributions_less_than_200":11941817.07,
         "contributions_200_499":779898.97,
         "contributions_500_1499":1791355.88,
         "contributions_1500_2699":672746.04,
         "contributions_2700":2332800.0,
         "net_primary":12329055.04,
         "net_primary_pct":46.400000000000006,
         "contributions_less_than_200_pct":45.4,
         "contributions_2700_pct":8.9,
         "burn_rate":57.020388584243584,
         "total_contributions":26316661.33
      },
      {
         "slug":"bush",
         "candidate_name":"Bush, Jeb",
         "name":"Jeb Bush",
         "party":"R",
         "committee":"committees/C00579458.json",
         "committee_id":"C00579458",
         "total_receipts":24814729.7,
         "total_disbursements":14543600.61,
         "cash_on_hand":10271129.09,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P60008059",
         "contributions_less_than_200":1377173.44,
         "contributions_200_499":386709.62,
         "contributions_500_1499":2618185.08,
         "contributions_1500_2699":1098067.44,
         "contributions_2700":18119700.0,
         "net_primary":10001259.29,
         "net_primary_pct":40.300000000000004,
         "contributions_less_than_200_pct":5.6000000000000005,
         "contributions_2700_pct":73.1,
         "burn_rate":85.66049427145371,
         "total_contributions":24779029.78
      },
      {
         "slug":"rubio",
         "candidate_name":"Rubio, Marco",
         "name":"Marco Rubio",
         "party":"R",
         "committee":"committees/C00458844.json",
         "committee_id":"C00458844",
         "total_receipts":14601652.83,
         "total_disbursements":7691528.12,
         "cash_on_hand":10975988.78,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P60006723",
         "contributions_less_than_200":3884623.6,
         "contributions_200_499":851290.79,
         "contributions_500_1499":2785561.0,
         "contributions_1500_2699":1634585.54,
         "contributions_2700":4549500.0,
         "net_primary":9738587.29,
         "net_primary_pct":66.7,
         "contributions_less_than_200_pct":27.0,
         "contributions_2700_pct":31.6,
         "burn_rate":80.48968834016155,
         "total_contributions":14404536.92
      },
      {
         "slug":"paul",
         "candidate_name":"Paul, Rand",
         "name":"Rand Paul",
         "party":"R",
         "committee":"committees/C00575449.json",
         "committee_id":"C00575449",
         "total_receipts":9442030.77,
         "total_disbursements":7317875.25,
         "cash_on_hand":2124155.52,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P40003576",
         "contributions_less_than_200":5021286.22,
         "contributions_200_499":766107.8,
         "contributions_500_1499":1344116.41,
         "contributions_1500_2699":730459.45,
         "contributions_2700":1026000.0,
         "net_primary":1763226.5,
         "net_primary_pct":18.7,
         "contributions_less_than_200_pct":64.7,
         "contributions_2700_pct":13.200000000000001,
         "burn_rate":181.19391437836785,
         "total_contributions":7763828.25
      },
      {
         "slug":"fiorina",
         "candidate_name":"Fiorina, Carly",
         "name":"Carly Fiorina",
         "party":"R",
         "committee":"committees/C00577312.json",
         "committee_id":"C00577312",
         "total_receipts":8496012.5,
         "total_disbursements":2946818.09,
         "cash_on_hand":5549194.41,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P60007242",
         "contributions_less_than_200":4470513.35,
         "contributions_200_499":440428.89,
         "contributions_500_1499":1698384.38,
         "contributions_1500_2699":434665.48,
         "contributions_2700":1231200.0,
         "net_primary":5524419.41,
         "net_primary_pct":65.0,
         "contributions_less_than_200_pct":52.6,
         "contributions_2700_pct":14.499999999999998,
         "burn_rate":32.876920029770524,
         "total_contributions":8493688.63
      },
      {
         "slug":"walker",
         "candidate_name":"Walker, Scott",
         "name":"Scott Walker",
         "party":"R",
         "committee":"committees/C00580480.json",
         "committee_id":"C00580480",
         "total_receipts":7379170.56,
         "total_disbursements":6393957.13,
         "cash_on_hand":985213.43,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-06-17",
         "candidate_id":"P60006046",
         "contributions_less_than_200":2652469.57,
         "contributions_200_499":0.0,
         "contributions_500_1499":0.0,
         "contributions_1500_2699":0.0,
         "contributions_2700":0.0,
         "net_primary":985213.43,
         "net_primary_pct":13.4,
         "contributions_less_than_200_pct":36.0,
         "contributions_2700_pct":0.0,
         "burn_rate":86.64872397257612,
         "total_contributions":7377551.11
      },
      {
         "slug":"trump",
         "candidate_name":"Trump, Donald J.",
         "name":"Donald J. Trump",
         "party":"R",
         "committee":"committees/C00580100.json",
         "committee_id":"C00580100",
         "total_receipts":5828922.1,
         "total_disbursements":5574149.22,
         "cash_on_hand":254772.88,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P80001571",
         "contributions_less_than_200":2870498.14,
         "contributions_200_499":276210.56,
         "contributions_500_1499":324821.14,
         "contributions_1500_2699":21400.0,
         "contributions_2700":321300.0,
         "net_primary":225762.96,
         "net_primary_pct":3.9,
         "contributions_less_than_200_pct":71.5,
         "contributions_2700_pct":8.0,
         "burn_rate":105.93308515969893,
         "total_contributions":4015115.15
      },
      {
         "slug":"graham",
         "candidate_name":"Graham, Lindsey",
         "name":"Lindsey Graham",
         "party":"R",
         "committee":"committees/C00578757.json",
         "committee_id":"C00578757",
         "total_receipts":4762210.55,
         "total_disbursements":3110901.42,
         "cash_on_hand":1651309.13,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P60007697",
         "contributions_less_than_200":433485.14,
         "contributions_200_499":161131.2,
         "contributions_500_1499":1114226.82,
         "contributions_1500_2699":1299757.18,
         "contributions_2700":1744200.0,
         "net_primary":1428481.43,
         "net_primary_pct":30.0,
         "contributions_less_than_200_pct":8.3,
         "contributions_2700_pct":33.4,
         "burn_rate":188.4912589147457,
         "total_contributions":5221751.64
      },
      {
         "slug":"kasich",
         "candidate_name":"Kasich, John",
         "name":"John Kasich",
         "party":"R",
         "committee":"committees/C00581876.json",
         "committee_id":"C00581876",
         "total_receipts":4376787.95,
         "total_disbursements":1734838.32,
         "cash_on_hand":2641949.63,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P60003670",
         "contributions_less_than_200":515378.23,
         "contributions_200_499":154978.0,
         "contributions_500_1499":564859.37,
         "contributions_1500_2699":233450.0,
         "contributions_2700":2743200.0,
         "net_primary":2641949.63,
         "net_primary_pct":60.4,
         "contributions_less_than_200_pct":11.799999999999999,
         "contributions_2700_pct":62.7,
         "burn_rate":39.6372485900305,
         "total_contributions":4374838.22
      },
      {
         "slug":"christie",
         "candidate_name":"Christie, Chris",
         "name":"Chris Christie",
         "party":"R",
         "committee":"committees/C00580399.json",
         "committee_id":"C00580399",
         "total_receipts":4208984.49,
         "total_disbursements":2822537.1,
         "cash_on_hand":1386447.39,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P60008521",
         "contributions_less_than_200":151326.26,
         "contributions_200_499":65611.0,
         "contributions_500_1499":509446.79,
         "contributions_1500_2699":238380.38,
         "contributions_2700":3123900.0,
         "net_primary":1386447.39,
         "net_primary_pct":32.9,
         "contributions_less_than_200_pct":3.5999999999999996,
         "contributions_2700_pct":74.2,
         "burn_rate":67.05981233017089,
         "total_contributions":4208164.43
      },
      {
         "slug":"omalley",
         "candidate_name":"O'Malley, Martin",
         "name":"Martin O'Malley",
         "party":"D",
         "committee":"committees/C00578658.json",
         "committee_id":"C00578658",
         "total_receipts":3289725.6,
         "total_disbursements":2483738.77,
         "cash_on_hand":805986.83,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P60007671",
         "contributions_less_than_200":247097.7,
         "contributions_200_499":119098.6,
         "contributions_500_1499":765361.55,
         "contributions_1500_2699":259555.0,
         "contributions_2700":1738800.0,
         "net_primary":805986.83,
         "net_primary_pct":24.5,
         "contributions_less_than_200_pct":7.6,
         "contributions_2700_pct":53.800000000000004,
         "burn_rate":139.61185712499918,
         "total_contributions":3234569.24
      },
      {
         "slug":"huckabee",
         "candidate_name":"Huckabee, Mike",
         "name":"Mike Huckabee",
         "party":"R",
         "committee":"committees/C00577981.json",
         "committee_id":"C00577981",
         "total_receipts":3246200.24,
         "total_disbursements":2484789.28,
         "cash_on_hand":761410.96,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P80003478",
         "contributions_less_than_200":1366574.36,
         "contributions_200_499":132632.01,
         "contributions_500_1499":535028.66,
         "contributions_1500_2699":314854.72,
         "contributions_2700":691200.0,
         "net_primary":761410.96,
         "net_primary_pct":23.5,
         "contributions_less_than_200_pct":42.1,
         "contributions_2700_pct":21.3,
         "burn_rate":109.99084339491363,
         "total_contributions":3245193.14
      },
      {
         "slug":"perry",
         "candidate_name":"Perry, Rick",
         "name":"Rick Perry",
         "party":"R",
         "committee":"committees/C00500587.json",
         "committee_id":"C00500587",
         "total_receipts":1426683.68,
         "total_disbursements":1781011.41,
         "cash_on_hand":44553.59,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P20003281",
         "contributions_less_than_200":146189.17,
         "contributions_200_499":45857.79,
         "contributions_500_1499":180029.04,
         "contributions_1500_2699":68900.0,
         "contributions_2700":888300.0,
         "net_primary":-17606.89,
         "net_primary_pct":-1.2,
         "contributions_less_than_200_pct":10.9,
         "contributions_2700_pct":66.5,
         "burn_rate":392.2568262616527,
         "total_contributions":1336316.18
      },
      {
         "slug":"jindal",
         "candidate_name":"Jindal, Bobby",
         "name":"Bobby Jindal",
         "party":"R",
         "committee":"committees/C00580159.json",
         "committee_id":"C00580159",
         "total_receipts":1158196.9,
         "total_disbursements":897257.89,
         "cash_on_hand":260939.01,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P60008398",
         "contributions_less_than_200":138830.9,
         "contributions_200_499":20616.0,
         "contributions_500_1499":63050.0,
         "contributions_1500_2699":40100.0,
         "contributions_2700":885600.0,
         "net_primary":255440.01,
         "net_primary_pct":22.1,
         "contributions_less_than_200_pct":12.0,
         "contributions_2700_pct":76.5,
         "burn_rate":143.62424622918064,
         "total_contributions":1158196.9
      },
      {
         "slug":"lessig",
         "candidate_name":"Lessig, Lawrence",
         "name":"Lawrence Lessig",
         "party":"D",
         "committee":"committees/C00583146.json",
         "committee_id":"C00583146",
         "total_receipts":1016189.22,
         "total_disbursements":442253.62,
         "cash_on_hand":573935.6,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-08-01",
         "candidate_id":"P60009685",
         "contributions_less_than_200":423556.88,
         "contributions_200_499":104595.05,
         "contributions_500_1499":177103.0,
         "contributions_1500_2699":18085.0,
         "contributions_2700":291600.0,
         "net_primary":573935.6,
         "net_primary_pct":56.49999999999999,
         "contributions_less_than_200_pct":41.699999999999996,
         "contributions_2700_pct":28.7,
         "burn_rate":43.52079428671759,
         "total_contributions":1015559.93
      },
      {
         "slug":"santorum",
         "candidate_name":"Santorum, Rick",
         "name":"Rick Santorum",
         "party":"R",
         "committee":"committees/C00578492.json",
         "committee_id":"C00578492",
         "total_receipts":995602.49,
         "total_disbursements":769076.57,
         "cash_on_hand":226525.92,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P20002721",
         "contributions_less_than_200":228850.05,
         "contributions_200_499":33351.09,
         "contributions_500_1499":172270.0,
         "contributions_1500_2699":103005.05,
         "contributions_2700":450900.0,
         "net_primary":204825.92,
         "net_primary_pct":20.599999999999998,
         "contributions_less_than_200_pct":23.0,
         "contributions_2700_pct":45.300000000000004,
         "burn_rate":101.41560474102351,
         "total_contributions":994976.19
      },
      {
         "slug":"webb",
         "candidate_name":"Webb, Jim",
         "name":"Jim Webb",
         "party":"D",
         "committee":"committees/C00581215.json",
         "committee_id":"C00581215",
         "total_receipts":696972.18,
         "total_disbursements":380206.84,
         "cash_on_hand":316765.34,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2014-11-20",
         "candidate_id":"P60008885",
         "contributions_less_than_200":307130.18,
         "contributions_200_499":38280.0,
         "contributions_500_1499":167179.0,
         "contributions_1500_2699":92983.0,
         "contributions_2700":81000.0,
         "net_primary":316765.34,
         "net_primary_pct":45.4,
         "contributions_less_than_200_pct":44.1,
         "contributions_2700_pct":11.600000000000001,
         "burn_rate":54.551221829255795,
         "total_contributions":696972.18
      },
      {
         "slug":"pataki",
         "candidate_name":"Pataki, George",
         "name":"George Pataki",
         "party":"R",
         "committee":"committees/C00578245.json",
         "committee_id":"C00578245",
         "total_receipts":409308.85,
         "total_disbursements":395738.3,
         "cash_on_hand":13570.55,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P60007572",
         "contributions_less_than_200":24937.55,
         "contributions_200_499":7975.0,
         "contributions_500_1499":63846.3,
         "contributions_1500_2699":30650.0,
         "contributions_2700":261900.0,
         "net_primary":13570.55,
         "net_primary_pct":3.3000000000000003,
         "contributions_less_than_200_pct":6.4,
         "contributions_2700_pct":67.30000000000001,
         "burn_rate":226.40530443206148,
         "total_contributions":389308.85
      },
      {
         "slug":"chafee",
         "candidate_name":"Chafee, Lincoln",
         "name":"Lincoln Chafee",
         "party":"D",
         "committee":"committees/C00579706.json",
         "committee_id":"C00579706",
         "total_receipts":408201.03,
         "total_disbursements":123674.76,
         "cash_on_hand":284526.27,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P60008075",
         "contributions_less_than_200":16583.0,
         "contributions_200_499":4340.0,
         "contributions_500_1499":9350.0,
         "contributions_1500_2699":4050.0,
         "contributions_2700":5400.0,
         "net_primary":284526.27,
         "net_primary_pct":69.69999999999999,
         "contributions_less_than_200_pct":37.3,
         "contributions_2700_pct":12.1,
         "burn_rate":387.61691463113266,
         "total_contributions":44506.89
      },
      {
         "slug":"gilmore",
         "candidate_name":"Gilmore, Jim",
         "name":"Jim Gilmore",
         "party":"R",
         "committee":"committees/C00582668.json",
         "committee_id":"C00582668",
         "total_receipts":105807.24,
         "total_disbursements":71422.84,
         "cash_on_hand":34384.4,
         "date_coverage_to":"2015-09-30",
         "date_coverage_from":"2015-07-01",
         "candidate_id":"P80003379",
         "contributions_less_than_200":757.24,
         "contributions_200_499":250.0,
         "contributions_500_1499":8500.0,
         "contributions_1500_2699":0.0,
         "contributions_2700":51300.0,
         "net_primary":34384.4,
         "net_primary_pct":32.5,
         "contributions_less_than_200_pct":1.2,
         "contributions_2700_pct":81.69999999999999,
         "burn_rate":67.50279092432616,
         "total_contributions":62807.24
      }
   ]
}
```

This endpoint retrieves totals (receipts and disbursements, plus several calculated values) for all presidential candidates for a particular campaign cycle (2008, 2012, 2016).

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/president/totals`

## Presidential Candidate Details

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "results":[
      {
         "candidate_name":"Rubio, Marco",
         "name":"Marco Rubio",
         "candidate_id":"P60006723",
         "committee_id":"C00458844",
         "party":"R",
         "total_receipts":14601652.83,
         "total_disbursements":7691528.12,
         "individual_contributions":20981270.42,
         "cash_on_hand":10975988.78,
         "net_individual_contributions":5527654.57,
         "net_party_contributions":101.68,
         "net_pac_contributions":0.0,
         "net_candidate_contributions":0.0,
         "total_contributions":14404536.92,
         "contributions_200_499":851290.79,
         "transfers_in":2694072.24,
         "federal_funds":0.0,
         "total_contributions_less_than_200":3884623.6,
         "total_contributions_max":4549500.0,
         "net_primary_contributions":9738587.29,
         "net_general_contributions":1237401.49,
         "total_refunds":1127716.53,
         "contributions_500_1499":2785561.0,
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-09-30",
         "cash_on_hand_party_rank":3,
         "total_receipts_party_rank":4,
         "total_disbursements_party_rank":4,
         "contributions_less_than_200_party_rank":3,
         "contributions_200_499_party_rank":3,
         "contributions_500_1499_party_rank":2,
         "contributions_1500_2699_party_rank":1,
         "contributions_max_party_rank":2,
         "contributions_1500_2699":1634585.54,
         "net_primary_party_rank":4,
         "contributions_max":4549500.0
      }
   ]
}
```

This endpoint retrieves campaign finance details for a particular presidential candidate in a particular campaign cycle (2008, 2012, 2016). Requests specify a candidate last name or a committee ID, but not both.

### HTTP Request

`GET http://api.propublica.org/campaign-finance/v1/{cycle}/president/candidates/{candidate-name | fec-id}`

### URL Parameters

Parameter | Description
--------- | -----------
candidate-name | The last name of a presidential candidate, in lower case.
fec-id | The FEC-assigned 9-character ID of a presidential candidate's main committee. To find a committee's official FEC ID, use the presidential totals request or the [FEC web site](http://www.fec.gov).
