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
