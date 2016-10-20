---
title: ProPublica Campaign Finance API Documentation

language_tabs:
  - shell

toc_footers:
  - <a href='mailto:apihelp@propublica.org'>Request an API Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>
  - © Copyright 2016 ProPublica Inc.

includes:
  - errors

search: true
---

# Campaign Finance API Documentation

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

The ProPublica Campaign Finance database is updated daily (electronic filings are updated every 15 minutes). For general information about FEC data, visit the [FEC Web site](http://www.fec.gov/disclosure.shtml).

### Legal

Usage constitues agreement to our [Data Terms of Use](https://www.propublica.org/about/propublica-data-terms-of-use). The data returned by the Campaign Finance API is subject to the [official sale and use restrictions](http://www.fec.gov/pages/brochures/sale_and_use_brochure.pdf) set by the Federal Election Commission. By accessing this data via the ProPublica API, you understand and acknowledge that you are using the data subject to all applicable local, state and federal law, including the FEC restrictions.


# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> Make sure to replace `PROPUBLICA_API_KEY` with your API key.

To use the Campaign Finance API, you must sign up for an API key by emailing [apihelp@propublica.org](mailto:apihelp@propublica.org). Usage is limited to 5000 requests per day (rate limits are subject to change). The API key must be included in all API requests to the server, set as a header:

`X-API-Key: PROPUBLICA_API_KEY`

<aside class="notice">
You must replace <code>PROPUBLICA_API_KEY</code> with your personal API key.
</aside>

# Requests

The Campaign Finance API uses a RESTful style. See individual methods below for permitted requests. The API only accepts GET requests. All requests begin with:

`https://api.propublica.org/campaign-finance/{version}/`

The current version is `v1`.

## Common Parameters

These parameters are used in all request types. See below for URI structures and additional parameters for each type of request.

  * *version* The API version (in URI path). The current version is `v1`.
  * *campaign-cycle* An even-numbered year between 1996 and 2016 (in URI path). The current cycle in `2016`.
  * *format* The format of the response (`.json` or `.xml`) added to the end of the request.

The following parameters are optional:  

  * *callback* JSONP callback function (in query string).
  * *offset* The first 20 results are shown by default for most requests. To page through the results, set offset to the appropriate multiple of 20. Date-based responses return all relevant results.

# Responses

The API provides JSON and XML responses for every type of request, and supports JSONP callbacks. Responses that are not date-based or containing aggregate totals return the first 20 results; pagination is available via an `offset` query string parameter using multiples of 20.

# Candidates

## Search for Candidates

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/candidates/search.json?query=Wilson"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 Pro Publica Inc. All Rights Reserved.",
   "cycle":2016,
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
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
         "state":"/races/IN.json",
         "district":"/races/IN/house/07.json"
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

This endpoint retrieves federal candidates by last name, using a query string parameter.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/candidates/search`

### Query Parameters

Parameter | Description
--------- | -----------
query | The first or last name of the candidate


## Get a Specific Candidate

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/candidates/P60005915.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
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
         "district":"/races/WA/house/05.json",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?H4WA05077",
         "committee":"/committees/C00390476.json",
         "state":"/races/WA.json",
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

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/candidates/{fec-id}`

### URL Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a candidate. To find a candidate's official FEC ID, use a candidate search request or the [FEC web site](http://www.fec.gov).


## Get Top 20 Candidates in Specific Financial Category

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/candidates/leaders/pac-total.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "cycle":2016,
   "category":"Contributions from PACs",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "results":[
      {
         "relative_uri":"/candidates/S4NC00089.json",
         "name":"BURR, RICHARD",
         "party":"REP",
         "state":"/races/NC.json",
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
         "state":"/races/NY.json",
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
         "state":"/races/OH.json",
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
         "state":"/races/CA.json",
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
         "state":"/races/PA.json",
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
         "state":"/races/MO.json",
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
         "state":"/races/GA.json",
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
         "state":"/races/CO.json",
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
         "state":"/races/WA.json",
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
         "state":"/races/SC.json",
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
         "state":"/races/SD.json",
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
         "state":"/races/AK.json",
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
         "state":"/races/WI.json",
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
         "state":"/races/OH.json",
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
         "state":"/races/IL.json",
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
         "state":"/races/FL.json",
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
         "state":"/races/IA.json",
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
         "state":"/races/PA.json",
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
         "state":"/races/KS.json",
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
         "state":"/races/ID.json",
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

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/candidates/leaders/{category}`

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

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/races/DE.json"
 -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "cycle":2016,
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
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
         "state":"/races/DE.json",
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
         "state":"/races/DE.json",
         "district":null
      }
   ]
}
```

This endpoint retrieves an array of FEC candidates for a given state (and optional chamber and district).

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/races/{state}/{chamber}/{district}`

### URL Parameters

Parameter | Description
--------- | -----------
state | Two-letter state abbreviation
chamber | `house` or `senate` (optional)
district | Specify the district number. Don't include for states with a single representative (AL, DE, DC, MT, ND, SD, VT). (House requests only - districts with Senate requests will be ignored.)

## Get Recently Added Candidates

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/candidates/new.json"
 -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "cycle":2016,
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
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
         "district":"/races/MN/house/06.json",
         "mailing_city":"ST PAUL ",
         "mailing_state":"MN",
         "mailing_zip":"55105",
         "party":"UNK",
         "state":"/races/MN.json",
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
         "district":"/races/NH/senate.json",
         "mailing_city":"MANCHESTER",
         "mailing_state":"NH",
         "mailing_zip":"03105",
         "party":"REP",
         "state":"/races/NH.json",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?S0NH00235",
         "committee":"/committees/C00464297.json"
      },
      {
         "id":"H0WI08075",
         "relative_uri":"/candidates/H0WI08075.json",
         "name":"RIBBLE, REID J. REP.",
         "district":"/races/WI/house/08.json",
         "mailing_city":"SHERWOOD",
         "mailing_state":"WI",
         "mailing_zip":"54169",
         "party":"REP",
         "state":"/races/WI.json",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?H0WI08075",
         "committee":"/committees/C00463620.json"
      },
      {
         "id":"S6CO00242",
         "relative_uri":"/candidates/S6CO00242.json",
         "name":"KINLAW, MICHAEL",
         "district":"/races/CO/senate.json",
         "mailing_city":"COLORADO SPRINGS",
         "mailing_state":"CO",
         "mailing_zip":"80949",
         "party":"REP",
         "state":"/races/CO.json",
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
         "district":"/races/ME/house/02.json",
         "mailing_city":"BANGOR",
         "mailing_state":"ME",
         "mailing_zip":"04402",
         "party":"DEM",
         "state":"/races/ME.json",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?H4ME02200",
         "committee":"/committees/C00546077.json"
      },
      {
         "id":"H6AL02142",
         "relative_uri":"/candidates/H6AL02142.json",
         "name":"GERRITSON, REBECCA (BECKY)",
         "district":"/races/AL/house/02.json",
         "mailing_city":"WETUMPKA",
         "mailing_state":"AL",
         "mailing_zip":"36902",
         "party":"REP",
         "state":"/races/AL.json",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?H6AL02142",
         "committee":"/committees/C00588244.json"
      }
   ]
}
```

This endpoint retrieves the 20 most recently added FEC candidates.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/candidates/new`

# Committees

## Search for Committees

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/committees/search.json?query=Americans%20for%20a%20Better"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "cycle":2016,
   "base_uri":"https://api.propublica.org/svc/elections/us/v3/finances/2016",
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

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/committees/search`

### Query Parameters

Parameter | Description
--------- | -----------
query | The name of the committee


## Get a Specific Committee

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/committees/C00553560.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "cycle":2016,
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
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

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/committees/{fec-id}`

### URL Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a committee. To find a committee's official FEC ID, use a committee search request or the [FEC web site](http://www.fec.gov).

## Get Recently Added Committees

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/committees/new.json"
 -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "cycle":2016,
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "results":[
      {
         "id":"C00577569",
         "relative_uri":"/committees/C00577569.json",
         "name":"TRUE CONSERVATIVES",
         "address":"PO BOX 26141",
         "city":"ALEXANDRIA",
         "state":"VA",
         "zip":"22313",
         "treasurer":"MARSTON, CHRIS",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577569/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":null,
         "receipt_date":"2015-05-07"
      },
      {
         "id":"C00577551",
         "relative_uri":"/committees/C00577551.json",
         "name":"IN GOD WE TRUST FOR TIMOTHY R FARKAS",
         "address":"291 E. LIVINGSTON AVE",
         "city":"COLUMBUS",
         "state":"OH",
         "zip":"43215",
         "treasurer":"SERGAKIS, CHRISTOPHER",
         "party":"IND",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577551/",
         "candidate":"/candidates/P60007416.json",
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"P",
         "filing_frequency":"Q",
         "committee_type":"P",
         "interest_group":"",
         "receipt_date":"2015-05-07"
      },
      {
         "id":"C00577445",
         "relative_uri":"/committees/C00577445.json",
         "name":"DEFEND OUR NATION PAC",
         "address":"203 SOUTH UNION STREET",
         "city":"ALEXANDRIA",
         "state":"VA",
         "zip":"22314",
         "treasurer":"MILLER, ROBERT L.",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577445/",
         "candidate":null,
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"V",
         "interest_group":"",
         "receipt_date":"2015-05-06"
      },
      {
         "id":"C00577460",
         "relative_uri":"/committees/C00577460.json",
         "name":"BILL OTTO FOR CONGRESS",
         "address":"11841 SOLOGNE COURT",
         "city":"MARYLAND HEIGHTS",
         "state":"MO",
         "zip":"63043",
         "treasurer":"FERDMAN, HARVEY MR.",
         "party":"DEM",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577460/",
         "candidate":null,
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"P",
         "filing_frequency":"Q",
         "committee_type":"H",
         "interest_group":"",
         "receipt_date":"2015-05-06"
      },
      {
         "id":"C00577452",
         "relative_uri":"/committees/C00577452.json",
         "name":"MAIN STREET PAC",
         "address":"POST OFFICE BOX 2",
         "city":"LIMERICK",
         "state":"ME",
         "zip":"04048",
         "treasurer":"LISA FEUERSTEIN",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577452/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":null,
         "receipt_date":"2015-05-06"
      },
      {
         "id":"C00577494",
         "relative_uri":"/committees/C00577494.json",
         "name":"DAVIS FOR CONGRESS",
         "address":"17 WEST COURTLAND STREET",
         "city":"BEL AIR",
         "state":"MD",
         "zip":"21014",
         "treasurer":"MADDOX, LEROY DANIEL JR",
         "party":"DEM",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577494/",
         "candidate":"/candidates/H6MD04233.json",
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"P",
         "filing_frequency":"Q",
         "committee_type":"H",
         "interest_group":"",
         "receipt_date":"2015-05-06"
      },
      {
         "id":"C00577486",
         "relative_uri":"/committees/C00577486.json",
         "name":"THE COMMITTEE FOR THE INSTALLATION OF LIMBERBUTT",
         "address":"8913 LIPPINCOTT RD",
         "city":"LOUISVILLE",
         "state":"KY",
         "zip":"40222",
         "treasurer":"WEISS, ISAAC",
         "party":"DEM",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577486/",
         "candidate":"/candidates/P60007366.json",
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"P",
         "filing_frequency":"Q",
         "committee_type":"P",
         "interest_group":"",
         "receipt_date":"2015-05-06"
      },
      {
         "id":"C00577544",
         "relative_uri":"/committees/C00577544.json",
         "name":"ILLINOIS SENATE VICTORY",
         "address":"120 MARYLAND AVE NE",
         "city":"WASHINGTON",
         "state":"DC",
         "zip":"20002",
         "treasurer":"YATES BAROODY",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/fecimg/?C00577544",
         "candidate":null,
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"J",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":"",
         "receipt_date":"2015-05-06"
      },
      {
         "id":"C00577478",
         "relative_uri":"/committees/C00577478.json",
         "name":"LINDA J HART",
         "address":"4312 CAPRA WAY ",
         "city":"BENBROOK",
         "state":"TX",
         "zip":"76126",
         "treasurer":"LINDA J HART",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577478/",
         "candidate":"/candidates/P60007317.json",
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"P",
         "filing_frequency":"Q",
         "committee_type":"P",
         "interest_group":"",
         "receipt_date":"2015-05-06"
      },
      {
         "id":"C00577395",
         "relative_uri":"/committees/C00577395.json",
         "name":"PEOPLE IN COMMAND/PIC",
         "address":"3674 RUNNYMEDE BLVD",
         "city":"CLEVELAND HTS",
         "state":"OH",
         "zip":"44121",
         "treasurer":"TRACEY SAPP",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577395/",
         "candidate":"/candidates/P00003392.json",
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"U",
         "interest_group":"",
         "receipt_date":"2015-05-05"
      },
      {
         "id":"C00577346",
         "relative_uri":"/committees/C00577346.json",
         "name":"CITIZENS FOR DANIEL P ZUTLER FOR U S PRESIDENT",
         "address":"7300 SEA GRAPE AVE",
         "city":"PORT RICHEY",
         "state":"FL",
         "zip":"34668",
         "treasurer":"REXFORD, BEVERLY ANN",
         "party":"IND",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577346/",
         "candidate":null,
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"A",
         "filing_frequency":"T",
         "committee_type":"P",
         "interest_group":null,
         "receipt_date":"2015-05-05"
      },
      {
         "id":"C00577411",
         "relative_uri":"/committees/C00577411.json",
         "name":"COMMITTEE TO ELECT LLOYD KELSO PRESIDENT",
         "address":"128 EAST GARRISON BLVD SUITE A",
         "city":"GASTONIA",
         "state":"NC",
         "zip":"28053",
         "treasurer":"DEBRA SETZER KELSO",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577411/",
         "candidate":"/candidates/P60007267.json",
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"P",
         "filing_frequency":"Q",
         "committee_type":"P",
         "interest_group":"",
         "receipt_date":"2015-05-05"
      },
      {
         "id":"C00577387",
         "relative_uri":"/committees/C00577387.json",
         "name":"HIGH SCHOOL DEMOCRATS OF AMERICA",
         "address":"4100 NORTH LINCOLN BOULEVARD",
         "city":"OKLAHOMA CITY",
         "state":"OK",
         "zip":"73105",
         "treasurer":"SARAH CLAYTON",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577387/",
         "candidate":null,
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":null,
         "receipt_date":"2015-05-05"
      },
      {
         "id":"C00577403",
         "relative_uri":"/committees/C00577403.json",
         "name":"TONY SMITHERMAN'S AFTER PARTY COMMITTEE",
         "address":"5432 MARSHALL ST",
         "city":"LUBBOCK",
         "state":"TX",
         "zip":"79416",
         "treasurer":"SMITHERMAN, TONY",
         "party":"OTH",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577403/",
         "candidate":"/candidates/P60007259.json",
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"P",
         "filing_frequency":"Q",
         "committee_type":"P",
         "interest_group":"",
         "receipt_date":"2015-05-05"
      },
      {
         "id":"C00577379",
         "relative_uri":"/committees/C00577379.json",
         "name":"USA STRONG PAC",
         "address":"49 NORTH FEDERAL HWY #104",
         "city":"POMPANO BEACH",
         "state":"FL",
         "zip":"33062",
         "treasurer":"DANIELLA ACOSTA",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577379/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":null,
         "receipt_date":"2015-05-05"
      },
      {
         "id":"C00577429",
         "relative_uri":"/committees/C00577429.json",
         "name":"MARTY PIATT FOR U.S. PRESIDENT CAMPAIGN COMMITTEE",
         "address":"1254 WOODHAVEN DRIVE",
         "city":"OCEANSIDE",
         "state":"CA",
         "zip":"92056",
         "treasurer":"PIATT, MARTY",
         "party":"UNK",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577429/",
         "candidate":"/candidates/P60007283.json",
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"P",
         "filing_frequency":"Q",
         "committee_type":"P",
         "interest_group":"",
         "receipt_date":"2015-05-05"
      },
      {
         "id":"C00577361",
         "relative_uri":"/committees/C00577361.json",
         "name":"MULLIN VICTORY FUND",
         "address":"332 W LEE HWY",
         "city":"WARRENTON",
         "state":"VA",
         "zip":"20186",
         "treasurer":"RALLS, STEVE",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577361/",
         "candidate":null,
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"J",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":"",
         "receipt_date":"2015-05-05"
      },
      {
         "id":"C00577270",
         "relative_uri":"/committees/C00577270.json",
         "name":"FLORES FOR CONGRESS",
         "address":"420 N NELLIS BLVD",
         "city":"LAS VEGAS",
         "state":"NV",
         "zip":"89110",
         "treasurer":"CISNEROS, NORBERTO",
         "party":"DEM",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577270/",
         "candidate":"/candidates/H6NV04012.json",
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"P",
         "filing_frequency":"Q",
         "committee_type":"H",
         "interest_group":"",
         "receipt_date":"2015-05-04"
      },
      {
         "id":"C00577247",
         "relative_uri":"/committees/C00577247.json",
         "name":"ABC",
         "address":"ABC",
         "city":"ABC",
         "state":"TX",
         "zip":"73344",
         "treasurer":"GUPTA, MUKUL",
         "party":"ACE",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577247/",
         "candidate":null,
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"P",
         "filing_frequency":"Q",
         "committee_type":"H",
         "interest_group":"",
         "receipt_date":"2015-05-04"
      },
      {
         "id":"C00577262",
         "relative_uri":"/committees/C00577262.json",
         "name":"LEAD THE WAY PAC",
         "address":"308 SAPLING COURT",
         "city":"CRANBERRY TOWNSHIP",
         "state":"PA",
         "zip":"16066",
         "treasurer":"COLEMAN, ANN",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00577262/",
         "candidate":null,
         "leadership":false,
         "super_pac":false,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"N",
         "interest_group":"",
         "receipt_date":"2015-05-04"
      }
   ]
}
```

This endpoint retrieves the 20 most recently added FEC committees.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/committees/new`

## Get Recently Added Independent Expenditure-Only Committees

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/committees/superpacs.json"
 -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "cycle":2016,
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "offset":null,
   "results":[
      {
         "id":"C00604496",
         "relative_uri":"/committees/C00604496.json",
         "name":"Americans United for Values",
         "address":"P.O. Box 90891",
         "city":"Washington",
         "state":"DC",
         "zip":"20090",
         "treasurer":"Kinnett, Brian",
         "party":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00604496/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":null,
         "filing_frequency":null,
         "committee_type":"F",
         "interest_group":null
      },
      {
         "id":"C00603688",
         "relative_uri":"/committees/C00603688.json",
         "name":"S.D.T.F.B",
         "address":"1011 hampton ct",
         "city":"kennesaw",
         "state":"GA",
         "zip":"30144",
         "treasurer":"Wachner, Kristofer John",
         "party":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00603688/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":null,
         "filing_frequency":null,
         "committee_type":"F",
         "interest_group":null
      },
      {
         "id":"C00603647",
         "relative_uri":"/committees/C00603647.json",
         "name":"Conservative Texans",
         "address":"873 Amarillo",
         "city":"Abilene",
         "state":"TX",
         "zip":"79602",
         "treasurer":"Barnett, Mitch",
         "party":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00603647/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":null,
         "filing_frequency":null,
         "committee_type":"F",
         "interest_group":null
      },
      {
         "id":"C00603621",
         "relative_uri":"/committees/C00603621.json",
         "name":"Our Principles PAC",
         "address":"P. O. Box 25046",
         "city":"Alexandria",
         "state":"VA",
         "zip":"22313",
         "treasurer":"Jodoin, Jamie C.",
         "party":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00603621/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":null,
         "filing_frequency":null,
         "committee_type":"F",
         "interest_group":null
      },
      {
         "id":"C00603530",
         "relative_uri":"/committees/C00603530.json",
         "name":"Conservative Patriot Fund",
         "address":"873 Amarillo",
         "city":"Abilene",
         "state":"TX",
         "zip":"79602",
         "treasurer":"Barnett, Mitch",
         "party":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00603530/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":null,
         "filing_frequency":null,
         "committee_type":"F",
         "interest_group":null
      },
      {
         "id":"C00603175",
         "relative_uri":"/committees/C00603175.json",
         "name":"One Day In America",
         "address":"1775 I Street, NW",
         "city":"Washingotn",
         "state":"DC",
         "zip":"20006",
         "treasurer":"Railey-Cisco, Alex-St. James Andrew",
         "party":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00603175/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":null,
         "filing_frequency":null,
         "committee_type":"F",
         "interest_group":null
      },
      {
         "id":"C00603159",
         "relative_uri":"/committees/C00603159.json",
         "name":"INDIANA JOBS NOW",
         "address":"PO BOX 9891",
         "city":"ARLINGTON",
         "state":"VA",
         "zip":"22219",
         "treasurer":"REISNER, MICHELE",
         "party":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00603159/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":null,
         "filing_frequency":null,
         "committee_type":"F",
         "interest_group":null
      },
      {
         "id":"C00602987",
         "relative_uri":"/committees/C00602987.json",
         "name":"CITIZENS AGAINST EXCESSIVE REGULATIONS",
         "address":"48 TINTLE ROAD",
         "city":"KINNELON",
         "state":"NJ",
         "zip":"07405",
         "treasurer":"BENJAMIN, BRIAN",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00602987/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":""
      },
      {
         "id":"C00602896",
         "relative_uri":"/committees/C00602896.json",
         "name":"RAMPART PAC",
         "address":"PO BOX 1171",
         "city":"ALEXANDRIA",
         "state":"VA",
         "zip":"22313",
         "treasurer":"KOCH, TIMOTHY A.",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00602896/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":""
      },
      {
         "id":"C00602649",
         "relative_uri":"/committees/C00602649.json",
         "name":"MARATHON PAC",
         "address":"%BULLDOG COMPLIANCE",
         "city":"BEVERLY",
         "state":"MA",
         "zip":"01915",
         "treasurer":"GANTT, CHARLES MR.",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00602649/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":""
      },
      {
         "id":"C00602623",
         "relative_uri":"/committees/C00602623.json",
         "name":"AMERICA SPEAKS PAC",
         "address":"1713 S.E. 40TH STREET",
         "city":"CAPE CORAL",
         "state":"FL",
         "zip":"33904",
         "treasurer":"COOLEY, WILLIAM",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00602623/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":""
      },
      {
         "id":"C00600767",
         "relative_uri":"/committees/C00600767.json",
         "name":"TUCKFRUMP.COM",
         "address":"PO BOX 10472",
         "city":"COLUMBUS",
         "state":"OH",
         "zip":"43201",
         "treasurer":"LUKE MONTGOMERY",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00600767/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":null
      },
      {
         "id":"C00600668",
         "relative_uri":"/committees/C00600668.json",
         "name":"FUNDEM",
         "address":"PO BOX 230536",
         "city":"LAS VEGAS",
         "state":"NV",
         "zip":"89105",
         "treasurer":"CASTILLO, KANOEKAPUWAILANI",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00600668/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":null
      },
      {
         "id":"C00599639",
         "relative_uri":"/committees/C00599639.json",
         "name":"AMERICAN INDIANS TRIBAL GOVERNMENT OF GEORGIA",
         "address":"1900 WEST OAKLAND PARK BLVD.",
         "city":"FORT LAUDERDALE",
         "state":"FL",
         "zip":"33310",
         "treasurer":"LAROSE, JOSHBUA",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00599639/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":""
      },
      {
         "id":"C00599456",
         "relative_uri":"/committees/C00599456.json",
         "name":"COLORADO CONSERVATIVE PAC",
         "address":"PO BOX 26141",
         "city":"ALEXANDRIA",
         "state":"VA",
         "zip":"22313",
         "treasurer":"MARSTON, CHRIS",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00599456/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":""
      },
      {
         "id":"C00599449",
         "relative_uri":"/committees/C00599449.json",
         "name":"MAKE AMERICA DANK AGAIN",
         "address":"420 DANK AVENUE",
         "city":"GARY",
         "state":"IN",
         "zip":"46042",
         "treasurer":"PURDY, MATTHEW ROBERT",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00599449/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":""
      },
      {
         "id":"C00599076",
         "relative_uri":"/committees/C00599076.json",
         "name":"RISING TIDE FLORIDA",
         "address":"601 PENNSYLVANIA AVENUE NW",
         "city":"WASHINGTON",
         "state":"DC",
         "zip":"20004",
         "treasurer":"DAVID M POWERS",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00599076/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":null
      },
      {
         "id":"C00599035",
         "relative_uri":"/committees/C00599035.json",
         "name":"RIVERSIDE COUNTY CITIZENS FOR SECURITY",
         "address":"5822 CRIGHTON DRIVE",
         "city":"DUBLIN",
         "state":"OH",
         "zip":"43016",
         "treasurer":"ROB PHILLIPS",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00599035/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":null
      },
      {
         "id":"C00599019",
         "relative_uri":"/committees/C00599019.json",
         "name":"GET SUPERPACED",
         "address":"1014 SOUTH CONCORD RD",
         "city":"WEST CHESTER",
         "state":"PA",
         "zip":"19362",
         "treasurer":"JACK DEMARCO",
         "party":"",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00599019/",
         "candidate":null,
         "leadership":false,
         "super_pac":true,
         "sponsor_name":null,
         "designation":"U",
         "filing_frequency":"Q",
         "committee_type":"O",
         "interest_group":null
      }
  ]
}
```

This endpoint retrieves the 20 most recently added FEC independent expenditure-only committees, known as "super PACs".

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/committees/superpacs`


## Get Committee Filings

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/committees/C00553560/filings.json"
 -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
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

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/committees/{fec-id}/filings`

### URL Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a committee. To find a committee's official FEC ID, use a committee search request or the [FEC web site](http://www.fec.gov).

## Get Leadership Committees

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/committees/leadership.json"
 -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "cycle":2016,
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
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

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/committees/leadership`

# Presidential Campaigns

## Presidential Candidate Totals

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/president/totals.json"
 -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
	"status": "OK",
	"base_uri": "http://propublica-campfin-cron.elasticbeanstalk.comcampaign-finance/v1/2016/",
	"cycle": 2016,
	"copyright": "Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
	"results": [{
		"slug": "clinton",
		"candidate_name": "Clinton, Hillary",
		"name": "Hillary Clinton",
		"party": "D",
		"committee": "committees/C00575795.json",
		"committee_id": "C00575795",
		"total_receipts": 186735031.61,
		"total_disbursements": 157763659.51,
		"cash_on_hand": 28971372.1,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-04-01",
		"candidate_id": "P00003392",
		"contributions_less_than_200": 51431956.48,
		"contributions_200_499": 11434374.12,
		"contributions_500_1499": 25107086.49,
		"contributions_1500_2699": 13644451.08,
		"contributions_max": 76523400.0,
		"net_primary": 24342711.9,
		"net_primary_pct": 13.0,
		"contributions_less_than_200_pct": 30.3,
		"contributions_max_pct": 45.1,
		"burn_rate": 106.9341168047289,
		"total_contributions": 169552497.08,
		"independent_expenditures_support": 10116597.97,
		"independent_expenditures_oppose": 6987601.95000001
	}, {
		"slug": "sanders",
		"candidate_name": "Sanders, Bernie",
		"name": "Bernie Sanders",
		"party": "D",
		"committee": "committees/C00577130.json",
		"committee_id": "C00577130",
		"total_receipts": 185864101.89,
		"total_disbursements": 168349117.67,
		"cash_on_hand": 17460961.37,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-04-01",
		"candidate_id": "P60007168",
		"contributions_less_than_200": 221735971.88,
		"contributions_200_499": 19404608.34,
		"contributions_500_1499": 20837551.49,
		"contributions_1500_2699": 3105827.9,
		"contributions_max": 2068200.0,
		"net_primary": 17460961.37,
		"net_primary_pct": 9.4,
		"contributions_less_than_200_pct": 121.2,
		"contributions_max_pct": 1.0999999999999999,
		"burn_rate": 99.45757021313398,
		"total_contributions": 182923991.19,
		"independent_expenditures_support": 4334387.32,
		"independent_expenditures_oppose": 859341.11
	}, {
		"slug": "cruz",
		"candidate_name": "Cruz, Ted",
		"name": "Ted Cruz",
		"party": "R",
		"committee": "committees/C00574624.json",
		"committee_id": "C00574624",
		"total_receipts": 79060950.56,
		"total_disbursements": 70254861.95,
		"cash_on_hand": 8806088.61,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-01-01",
		"candidate_id": "P60006111",
		"contributions_less_than_200": 49809054.51,
		"contributions_200_499": 7008975.51,
		"contributions_500_1499": 8823012.37,
		"contributions_1500_2699": 2282803.11,
		"contributions_max": 7263000.0,
		"net_primary": 4652997.16,
		"net_primary_pct": 5.8999999999999995,
		"contributions_less_than_200_pct": 63.3,
		"contributions_max_pct": 9.2,
		"burn_rate": 93.926460079587,
		"total_contributions": 78679628.91,
		"independent_expenditures_support": 20653943.7,
		"independent_expenditures_oppose": 9231692.41
	}, {
		"slug": "carson",
		"candidate_name": "Carson, Ben",
		"name": "Ben Carson",
		"party": "R",
		"committee": "committees/C00573519.json",
		"committee_id": "C00573519",
		"total_receipts": 64101879.28,
		"total_disbursements": 60735508.43,
		"cash_on_hand": 3366370.85,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-01-01",
		"candidate_id": "P60005915",
		"contributions_less_than_200": 47118584.66,
		"contributions_200_499": 5010794.25,
		"contributions_500_1499": 7428252.61,
		"contributions_1500_2699": 1228302.13,
		"contributions_max": 1952100.0,
		"net_primary": 2532820.62,
		"net_primary_pct": 4.0,
		"contributions_less_than_200_pct": 74.2,
		"contributions_max_pct": 3.1,
		"burn_rate": 403.7636261097707,
		"total_contributions": 63469219.92,
		"independent_expenditures_support": 4700711.12,
		"independent_expenditures_oppose": 2910.7
	}, {
		"slug": "rubio",
		"candidate_name": "Rubio, Marco",
		"name": "Marco Rubio",
		"party": "R",
		"committee": "committees/C00458844.json",
		"committee_id": "C00458844",
		"total_receipts": 56650061.41,
		"total_disbursements": 53455375.79,
		"cash_on_hand": 3736205.11,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-04-01",
		"candidate_id": "P60006723",
		"contributions_less_than_200": 14245863.39,
		"contributions_200_499": 2928690.61,
		"contributions_500_1499": 8346507.91,
		"contributions_1500_2699": 4053985.54,
		"contributions_max": 13535100.0,
		"net_primary": 791439.59,
		"net_primary_pct": 1.4000000000000001,
		"contributions_less_than_200_pct": 26.8,
		"contributions_max_pct": 25.4,
		"burn_rate": 166.18885723607215,
		"total_contributions": 53216332.09,
		"independent_expenditures_support": 39675849.74,
		"independent_expenditures_oppose": 7591247.66999999
	}, {
		"slug": "trump",
		"candidate_name": "Trump, Donald J.",
		"name": "Donald J. Trump",
		"party": "R",
		"committee": "committees/C00580100.json",
		"committee_id": "C00580100",
		"total_receipts": 49296493.8,
		"total_disbursements": 47185423.71,
		"cash_on_hand": 2111070.09,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-04-02",
		"candidate_id": "P80001571",
		"contributions_less_than_200": 9641910.28,
		"contributions_200_499": 875672.44,
		"contributions_500_1499": 753579.05,
		"contributions_1500_2699": 85870.58,
		"contributions_max": 747900.0,
		"net_primary": 2036721.63,
		"net_primary_pct": 4.1000000000000005,
		"contributions_less_than_200_pct": 76.8,
		"contributions_max_pct": 6.0,
		"burn_rate": 94.7081997541097,
		"total_contributions": 12562427.5,
		"independent_expenditures_support": 1516288.02,
		"independent_expenditures_oppose": 52104202.9899999
	}, {
		"slug": "bush",
		"candidate_name": "Bush, Jeb",
		"name": "Jeb Bush",
		"party": "R",
		"committee": "committees/C00579458.json",
		"committee_id": "C00579458",
		"total_receipts": 35224251.05,
		"total_disbursements": 35193065.7,
		"cash_on_hand": 31185.35,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-04-01",
		"candidate_id": "P60008059",
		"contributions_less_than_200": 2490899.04,
		"contributions_200_499": 686152.3,
		"contributions_500_1499": 4432996.83,
		"contributions_1500_2699": 1904747.44,
		"contributions_max": 23444100.0,
		"net_primary": -529579.95,
		"net_primary_pct": -1.5,
		"contributions_less_than_200_pct": 7.199999999999999,
		"contributions_max_pct": 68.2,
		"burn_rate": 182.24781264481385,
		"total_contributions": 34362074.58,
		"independent_expenditures_support": 78108743.8,
		"independent_expenditures_oppose": 3004092.01
	}, {
		"slug": "kasich",
		"candidate_name": "Kasich, John",
		"name": "John Kasich",
		"party": "R",
		"committee": "committees/C00581876.json",
		"committee_id": "C00581876",
		"total_receipts": 16556203.26,
		"total_disbursements": 15396101.0,
		"cash_on_hand": 1160028.36,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-07-01",
		"candidate_id": "P60003670",
		"contributions_less_than_200": 4148222.0,
		"contributions_200_499": 1140401.49,
		"contributions_500_1499": 3025500.1,
		"contributions_1500_2699": 1008578.59,
		"contributions_max": 6806700.0,
		"net_primary": 713019.31,
		"net_primary_pct": 4.3,
		"contributions_less_than_200_pct": 25.1,
		"contributions_max_pct": 41.199999999999996,
		"burn_rate": 102.15124876431459,
		"total_contributions": 16507136.8,
		"independent_expenditures_support": 14545094.26,
		"independent_expenditures_oppose": 8124161.39
	}, {
		"slug": "paul",
		"candidate_name": "Paul, Rand",
		"name": "Rand Paul",
		"party": "R",
		"committee": "committees/C00575449.json",
		"committee_id": "C00575449",
		"total_receipts": 12255894.03,
		"total_disbursements": 12063217.44,
		"cash_on_hand": 192676.59,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-04-01",
		"candidate_id": "P40003576",
		"contributions_less_than_200": 6652547.99,
		"contributions_200_499": 1033049.3,
		"contributions_500_1499": 1719877.39,
		"contributions_1500_2699": 894325.37,
		"contributions_max": 1363500.0,
		"net_primary": -356896.91,
		"net_primary_pct": -2.9000000000000004,
		"contributions_less_than_200_pct": 64.7,
		"contributions_max_pct": 13.3,
		"burn_rate": -90500.76407247405,
		"total_contributions": 10276232.12,
		"independent_expenditures_support": 5316973.57,
		"independent_expenditures_oppose": 5498.01
	}, {
		"slug": "fiorina",
		"candidate_name": "Fiorina, Carly",
		"name": "Carly Fiorina",
		"party": "R",
		"committee": "committees/C00577312.json",
		"committee_id": "C00577312",
		"total_receipts": 12079590.52,
		"total_disbursements": 10713449.72,
		"cash_on_hand": 1366140.8,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-04-01",
		"candidate_id": "P60007242",
		"contributions_less_than_200": 6853636.43,
		"contributions_200_499": 632520.89,
		"contributions_500_1499": 2230554.84,
		"contributions_1500_2699": 536316.61,
		"contributions_max": 1557900.0,
		"net_primary": 1341365.8,
		"net_primary_pct": 11.1,
		"contributions_less_than_200_pct": 56.8,
		"contributions_max_pct": 12.9,
		"burn_rate": -427042.63236232195,
		"total_contributions": 12066919.88,
		"independent_expenditures_support": 3778864.56000001,
		"independent_expenditures_oppose": 171.01
	}, {
		"slug": "christie",
		"candidate_name": "Christie, Chris",
		"name": "Chris Christie",
		"party": "R",
		"committee": "committees/C00580399.json",
		"committee_id": "C00580399",
		"total_receipts": 8457440.55,
		"total_disbursements": 8303135.42,
		"cash_on_hand": 154305.13,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-07-01",
		"candidate_id": "P60008521",
		"contributions_less_than_200": 471329.97,
		"contributions_200_499": 146110.44,
		"contributions_500_1499": 1208283.04,
		"contributions_1500_2699": 704305.38,
		"contributions_max": 5589000.0,
		"net_primary": 154305.13,
		"net_primary_pct": 1.7999999999999998,
		"contributions_less_than_200_pct": 5.6000000000000005,
		"contributions_max_pct": 66.5,
		"burn_rate": 416.2465998182756,
		"total_contributions": 8406266.85,
		"independent_expenditures_support": 19720224.82,
		"independent_expenditures_oppose": 3574877.09
	}, {
		"slug": "walker",
		"candidate_name": "Walker, Scott",
		"name": "Scott Walker",
		"party": "R",
		"committee": "committees/C00580480.json",
		"committee_id": "C00580480",
		"total_receipts": 8332482.32,
		"total_disbursements": 8306967.74,
		"cash_on_hand": 25514.58,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-06-17",
		"candidate_id": "P60006046",
		"contributions_less_than_200": 2947511.54,
		"contributions_200_499": 450183.5,
		"contributions_500_1499": 1246333.38,
		"contributions_1500_2699": 328401.4,
		"contributions_max": 2365200.0,
		"net_primary": -371255.42,
		"net_primary_pct": -4.5,
		"contributions_less_than_200_pct": 36.3,
		"contributions_max_pct": 29.2,
		"burn_rate": 140.92414241205162,
		"total_contributions": 8111344.4,
		"independent_expenditures_support": 2259877.05,
		"independent_expenditures_oppose": 7031.49
	}, {
		"slug": "omalley",
		"candidate_name": "O'Malley, Martin",
		"name": "Martin O'Malley",
		"party": "D",
		"committee": "committees/C00578658.json",
		"committee_id": "C00578658",
		"total_receipts": 6244905.2,
		"total_disbursements": 6196144.62,
		"cash_on_hand": 48685.58,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-04-01",
		"candidate_id": "P60007671",
		"contributions_less_than_200": 620840.42,
		"contributions_200_499": 270264.0,
		"contributions_500_1499": 1117002.58,
		"contributions_1500_2699": 387055.0,
		"contributions_max": 2068200.0,
		"net_primary": 48685.58,
		"net_primary_pct": 0.8,
		"contributions_less_than_200_pct": 13.5,
		"contributions_max_pct": 44.9,
		"burn_rate": 135.00242717481763,
		"total_contributions": 4607695.33,
		"independent_expenditures_support": 361363.57,
		"independent_expenditures_oppose": 0.0
	}, {
		"slug": "graham",
		"candidate_name": "Graham, Lindsey",
		"name": "Lindsey Graham",
		"party": "R",
		"committee": "committees/C00578757.json",
		"committee_id": "C00578757",
		"total_receipts": 5716043.86,
		"total_disbursements": 5641870.37,
		"cash_on_hand": 74173.49,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-01-14",
		"candidate_id": "P60007697",
		"contributions_less_than_200": 319210.82,
		"contributions_200_499": 129907.2,
		"contributions_500_1499": 834292.36,
		"contributions_1500_2699": 863385.56,
		"contributions_max": 1425600.0,
		"net_primary": -161506.51,
		"net_primary_pct": -2.8000000000000003,
		"contributions_less_than_200_pct": 8.6,
		"contributions_max_pct": 38.5,
		"burn_rate": 75.26471877282688,
		"total_contributions": 3699324.54,
		"independent_expenditures_support": 3489313.70000001,
		"independent_expenditures_oppose": 1997.21
	}, {
		"slug": "huckabee",
		"candidate_name": "Huckabee, Mike",
		"name": "Mike Huckabee",
		"party": "R",
		"committee": "committees/C00577981.json",
		"committee_id": "C00577981",
		"total_receipts": 4307029.7,
		"total_disbursements": 4250680.43,
		"cash_on_hand": 56349.27,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-04-01",
		"candidate_id": "P80003478",
		"contributions_less_than_200": 2078322.01,
		"contributions_200_499": 188780.01,
		"contributions_500_1499": 643065.66,
		"contributions_1500_2699": 407962.72,
		"contributions_max": 810000.0,
		"net_primary": 56349.27,
		"net_primary_pct": 1.3,
		"contributions_less_than_200_pct": 48.3,
		"contributions_max_pct": 18.8,
		"burn_rate": 142.8090750281477,
		"total_contributions": 4298758.99,
		"independent_expenditures_support": 2813135.09,
		"independent_expenditures_oppose": 112754.03
	}, {
		"slug": "jindal",
		"candidate_name": "Jindal, Bobby",
		"name": "Bobby Jindal",
		"party": "R",
		"committee": "committees/C00580159.json",
		"committee_id": "C00580159",
		"total_receipts": 1442463.52,
		"total_disbursements": 1442463.52,
		"cash_on_hand": 0.0,
		"date_coverage_to": "2015-12-31",
		"date_coverage_from": "2015-04-01",
		"candidate_id": "P60008398",
		"contributions_less_than_200": 164797.52,
		"contributions_200_499": 24416.0,
		"contributions_500_1499": 86150.0,
		"contributions_1500_2699": 87900.0,
		"contributions_max": 1069200.0,
		"net_primary": 0.0,
		"net_primary_pct": 0.0,
		"contributions_less_than_200_pct": 11.4,
		"contributions_max_pct": 74.1,
		"burn_rate": 191.79375686107642,
		"total_contributions": 1442463.52,
		"independent_expenditures_support": 2634873.26,
		"independent_expenditures_oppose": 0.0
	}, {
		"slug": "perry",
		"candidate_name": "Perry, Rick",
		"name": "Rick Perry",
		"party": "R",
		"committee": "committees/C00500587.json",
		"committee_id": "C00500587",
		"total_receipts": 1427250.78,
		"total_disbursements": 1824314.05,
		"cash_on_hand": 1818.05,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-04-01",
		"candidate_id": "P20003281",
		"contributions_less_than_200": 146189.17,
		"contributions_200_499": 45857.79,
		"contributions_500_1499": 180029.04,
		"contributions_1500_2699": 68900.0,
		"contributions_max": 888300.0,
		"net_primary": -60342.43,
		"net_primary_pct": -4.2,
		"contributions_less_than_200_pct": 10.9,
		"contributions_max_pct": 66.5,
		"burn_rate": null,
		"total_contributions": 1336316.18,
		"independent_expenditures_support": 2373876.0,
		"independent_expenditures_oppose": 0.0
	}, {
		"slug": "santorum",
		"candidate_name": "Santorum, Rick",
		"name": "Rick Santorum",
		"party": "R",
		"committee": "committees/C00578492.json",
		"committee_id": "C00578492",
		"total_receipts": 1389775.93,
		"total_disbursements": 1386199.63,
		"cash_on_hand": 3576.3,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-04-01",
		"candidate_id": "P20002721",
		"contributions_less_than_200": 346621.68,
		"contributions_200_499": 62774.48,
		"contributions_500_1499": 247692.0,
		"contributions_1500_2699": 133875.05,
		"contributions_max": 567000.0,
		"net_primary": -12313.7,
		"net_primary_pct": -0.8999999999999999,
		"contributions_less_than_200_pct": 25.5,
		"contributions_max_pct": 41.8,
		"burn_rate": 325.4091019566976,
		"total_contributions": 1358013.21,
		"independent_expenditures_support": 165936.77,
		"independent_expenditures_oppose": 0.0
	}, {
		"slug": "lessig",
		"candidate_name": "Lessig, Lawrence",
		"name": "Lawrence Lessig",
		"party": "D",
		"committee": "committees/C00583146.json",
		"committee_id": "C00583146",
		"total_receipts": 1016189.22,
		"total_disbursements": 442253.62,
		"cash_on_hand": 573935.6,
		"date_coverage_to": "2015-09-30",
		"date_coverage_from": "2015-08-01",
		"candidate_id": "P60009685",
		"contributions_less_than_200": 423556.88,
		"contributions_200_499": 104595.05,
		"contributions_500_1499": 177103.0,
		"contributions_1500_2699": 18085.0,
		"contributions_max": 291600.0,
		"net_primary": 451624.62,
		"net_primary_pct": 44.4,
		"contributions_less_than_200_pct": 41.699999999999996,
		"contributions_max_pct": 28.7,
		"burn_rate": 43.52079428671759,
		"total_contributions": 1015559.93,
		"independent_expenditures_support": 0.0,
		"independent_expenditures_oppose": 0.0
	}, {
		"slug": "webb",
		"candidate_name": "Webb, Jim",
		"name": "Jim Webb",
		"party": "D",
		"committee": "committees/C00581215.json",
		"committee_id": "C00581215",
		"total_receipts": 776958.59,
		"total_disbursements": 734525.62,
		"cash_on_hand": 59350.21,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2014-11-20",
		"candidate_id": "P60008885",
		"contributions_less_than_200": 352227.43,
		"contributions_200_499": 46080.0,
		"contributions_500_1499": 183688.0,
		"contributions_1500_2699": 94983.0,
		"contributions_max": 89100.0,
		"net_primary": 59150.21,
		"net_primary_pct": 7.6,
		"contributions_less_than_200_pct": 45.300000000000004,
		"contributions_max_pct": 11.5,
		"burn_rate": 1240.1085755851032,
		"total_contributions": 776708.59,
		"independent_expenditures_support": 0.0,
		"independent_expenditures_oppose": 0.0
	}, {
		"slug": "chafee",
		"candidate_name": "Chafee, Lincoln",
		"name": "Lincoln Chafee",
		"party": "D",
		"committee": "committees/C00579706.json",
		"committee_id": "C00579706",
		"total_receipts": 418135.75,
		"total_disbursements": 418163.8,
		"cash_on_hand": -28.05,
		"date_coverage_to": "2015-12-31",
		"date_coverage_from": "2015-01-09",
		"candidate_id": "P60008075",
		"contributions_less_than_200": 21152.0,
		"contributions_200_499": 4340.0,
		"contributions_500_1499": 10350.0,
		"contributions_1500_2699": 4050.0,
		"contributions_max": 5400.0,
		"net_primary": 0.0,
		"net_primary_pct": 0.0,
		"contributions_less_than_200_pct": 38.9,
		"contributions_max_pct": 9.9,
		"burn_rate": 2964.2409650196482,
		"total_contributions": 54441.61,
		"independent_expenditures_support": 0.0,
		"independent_expenditures_oppose": 0.0
	}, {
		"slug": "gilmore",
		"candidate_name": "Gilmore, Jim",
		"name": "Jim Gilmore",
		"party": "R",
		"committee": "committees/C00582668.json",
		"committee_id": "C00582668",
		"total_receipts": 383453.44,
		"total_disbursements": 383311.01,
		"cash_on_hand": 142.43,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-07-01",
		"candidate_id": "P80003379",
		"contributions_less_than_200": 2797.04,
		"contributions_200_499": 3040.91,
		"contributions_500_1499": 22500.0,
		"contributions_1500_2699": 6500.0,
		"contributions_max": 67500.0,
		"net_primary": 142.43,
		"net_primary_pct": 0.0,
		"contributions_less_than_200_pct": 2.7,
		"contributions_max_pct": 64.7,
		"burn_rate": 119.82988882511997,
		"total_contributions": 104337.95,
		"independent_expenditures_support": 0.0,
		"independent_expenditures_oppose": 0.0
	}, {
		"slug": "pataki",
		"candidate_name": "Pataki, George",
		"name": "George Pataki",
		"party": "R",
		"committee": "committees/C00578245.json",
		"committee_id": "C00578245",
		"total_receipts": 1109.0,
		"total_disbursements": 9740.09,
		"cash_on_hand": 5301.24,
		"date_coverage_to": "2016-03-31",
		"date_coverage_from": "2015-04-01",
		"candidate_id": "P60007572",
		"contributions_less_than_200": 27810.05,
		"contributions_200_499": 19750.0,
		"contributions_500_1499": 81346.3,
		"contributions_1500_2699": 58150.0,
		"contributions_max": 315900.0,
		"net_primary": 5301.24,
		"net_primary_pct": 478.0,
		"contributions_less_than_200_pct": 2507.7000000000003,
		"contributions_max_pct": 28485.1,
		"burn_rate": 878.2768259693418,
		"total_contributions": 1109.0,
		"independent_expenditures_support": 118778.36,
		"independent_expenditures_oppose": 0.0
	}]
}
```

This endpoint retrieves totals (receipts and disbursements, plus several calculated values) for all presidential candidates for a particular campaign cycle (2008, 2012, 2016).

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/president/totals`

## Presidential Candidate Details

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/president/candidates/C00458844.json"
 -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
	"status": "OK",
	"copyright": "Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
	"results": [{
		"candidate_name": "Rubio, Marco",
		"name": "Marco Rubio",
		"candidate_id": "P60006723",
		"committee_id": "C00458844",
		"party": "R",
		"total_receipts": 56650061.41,
		"total_disbursements": 53455375.79,
		"individual_contributions": 51749870.35,
		"cash_on_hand": 3736205.11,
		"net_individual_contributions": 44079382.76,
		"net_party_contributions": 101.68,
		"net_pac_contributions": 0.0,
		"net_candidate_contributions": 0.0,
		"total_contributions": 53216332.09,
		"contributions_200_499": 2928690.61,
		"transfers_in": 2700162.39,
		"federal_funds": 0.0,
		"total_contributions_less_than_200": 14245863.39,
		"total_contributions_max": 13535100.0,
		"net_primary_contributions": 791439.59,
		"net_general_contributions": 2944765.52,
		"total_refunds": 1383507.33,
		"contributions_500_1499": 8346507.91,
		"date_coverage_from": "2015-04-01",
		"date_coverage_to": "2016-03-31",
		"cash_on_hand_party_rank": 2,
		"total_receipts_party_rank": 3,
		"total_disbursements_party_rank": 3,
		"contributions_less_than_200_party_rank": 3,
		"contributions_200_499_party_rank": 3,
		"contributions_500_1499_party_rank": 2,
		"contributions_1500_2699_party_rank": 1,
		"contributions_max_party_rank": 2,
		"contributions_1500_2699": 4053985.54,
		"net_primary_party_rank": 5,
		"contributions_max": 13535100.0,
		"independent_expenditures_support": 39675849.74,
		"independent_expenditures_oppose": 7591247.66999999
	}]
}
```

This endpoint retrieves campaign finance details for a particular presidential candidate in a particular campaign cycle (2008, 2012, 2016). Requests specify a candidate last name or a committee ID, but not both.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/president/candidates/{candidate-name | fec-id}`

### URL Parameters

Parameter | Description
--------- | -----------
candidate-name | The last name of a presidential candidate, in lower case.
fec-id | The FEC-assigned 9-character ID of a presidential candidate's main committee. To find a committee's official FEC ID, use the presidential totals request or the [FEC web site](http://www.fec.gov). Use _either_ a candidate's name or an FEC ID, but not both.

## Presidential State/Zip Code Totals

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/president/states/NM.json"
 -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "results":[
      {
         "candidate":"/candidates/P00003392.json",
         "full_name":"Hillary Clinton",
         "party":"D",
         "state":"NM",
         "total":"346076.73",
         "contribution_count":287
      },
      {
         "candidate":"/candidates/P60006111.json",
         "full_name":"Ted Cruz",
         "party":"R",
         "state":"NM",
         "total":"93556.0",
         "contribution_count":142
      },
      {
         "candidate":"/candidates/P60007168.json",
         "full_name":"Bernie Sanders",
         "party":"D",
         "state":"NM",
         "total":"63934.25",
         "contribution_count":129
      },
      {
         "candidate":"/candidates/P60005915.json",
         "full_name":"Ben Carson",
         "party":"R",
         "state":"NM",
         "total":"43103.0",
         "contribution_count":109
      },
      {
         "candidate":"/candidates/P60007242.json",
         "full_name":"Carly Fiorina",
         "party":"R",
         "state":"NM",
         "total":"19850.0",
         "contribution_count":32
      },
      {
         "candidate":"/candidates/P40003576.json",
         "full_name":"Rand Paul",
         "party":"R",
         "state":"NM",
         "total":"18033.0",
         "contribution_count":31
      },
      {
         "candidate":"/candidates/P60008059.json",
         "full_name":"Jeb Bush",
         "party":"R",
         "state":"NM",
         "total":"16050.0",
         "contribution_count":16
      },
      {
         "candidate":"/candidates/P60006723.json",
         "full_name":"Marco Rubio",
         "party":"R",
         "state":"NM",
         "total":"9250.0",
         "contribution_count":19
      },
      {
         "candidate":"/candidates/P60003670.json",
         "full_name":"John Kasich",
         "party":"R",
         "state":"NM",
         "total":"8400.0",
         "contribution_count":6
      },
      {
         "candidate":"/candidates/P60007671.json",
         "full_name":"Martin O'Malley",
         "party":"D",
         "state":"NM",
         "total":"4550.0",
         "contribution_count":6
      },
      {
         "candidate":"/candidates/P80001571.json",
         "full_name":"Donald J. Trump",
         "party":"R",
         "state":"NM",
         "total":"2290.11",
         "contribution_count":6
      },
      {
         "candidate":"/candidates/P60008885.json",
         "full_name":"Jim Webb",
         "party":"D",
         "state":"NM",
         "total":"1500.0",
         "contribution_count":3
      },
      {
         "candidate":"/candidates/P80003478.json",
         "full_name":"Mike Huckabee",
         "party":"R",
         "state":"NM",
         "total":"1000.0",
         "contribution_count":2
      },
      {
         "candidate":"/candidates/P20003281.json",
         "full_name":"Rick Perry",
         "party":"R",
         "state":"NM",
         "total":"500.0",
         "contribution_count":1
      },
      {
         "candidate":"/candidates/P60008398.json",
         "full_name":"Bobby Jindal",
         "party":"R",
         "state":"NM",
         "total":"500.0",
         "contribution_count":2
      },
      {
         "candidate":"/candidates/P60007572.json",
         "full_name":"George Pataki",
         "party":"R",
         "state":"NM",
         "total":"500.0",
         "contribution_count":1
      },
      {
         "candidate":"/candidates/P60007697.json",
         "full_name":"Lindsey Graham",
         "party":"R",
         "state":"NM",
         "total":"500.0",
         "contribution_count":1
      },
      {
         "candidate":"/candidates/P60009685.json",
         "full_name":"Lawrence Lessig",
         "party":"D",
         "state":"NM",
         "total":"250.0",
         "contribution_count":1
      }
   ],
   "offset":0
}
```

This endpoint retrieves total individual itemized contributions to presidential candidates for a particular state or zip code.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/president/{resource-type}/{location}`

### URL Parameters

Parameter | Description
--------- | -----------
resource-type | `states` or `zips`
location | Two-letter state abbreviation (for the `states` resource type) or five-digit ZIP code (for the `zips` resource type)

# Electronic Filings

## Search for Electronic Filings

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/filings/search.json?query=united%20egg"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "date":null,
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "offset":null,
   "results":[
      {
         "filing_id":1039997,
         "fec_committee_id":"C00172841",
         "committee":"/committees/C00172841.json",
         "committee_name":"UNITED EGG ASSOCIATION EGGPAC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-15",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00172841/1039997/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":95491.04,
         "cash_on_hand":310410.05,
         "disbursements_total":43004.13,
         "receipts_total":95560.7,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1039083,
         "fec_committee_id":"C00172841",
         "committee":"/committees/C00172841.json",
         "committee_name":"UNITED EGG ASSOCIATION EGGPAC",
         "form_type":"F99",
         "report_title":"MISCELLANEOUS DOCUMENT",
         "date_filed":"2016-01-12",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00172841/1039083/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1012939,
         "fec_committee_id":"C00172841",
         "committee":"/committees/C00172841.json",
         "committee_name":"UNITED EGG ASSOCIATION EGGPAC",
         "form_type":"F3",
         "report_title":"MID-YEAR",
         "date_filed":"2015-07-09",
         "date_coverage_from":"2015-01-01",
         "date_coverage_to":"2015-06-30",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00172841/1012939/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":630.0,
         "cash_on_hand":257853.48,
         "disbursements_total":46563.93,
         "receipts_total":700.79,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":982496,
         "fec_committee_id":"C00172841",
         "committee":"/committees/C00172841.json",
         "committee_name":"UNITED EGG ASSOCIATION EGGPAC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2015-01-07",
         "date_coverage_from":"2014-11-25",
         "date_coverage_to":"2014-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00172841/982496/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":6000.0,
         "cash_on_hand":303716.62,
         "disbursements_total":1138.61,
         "receipts_total":6012.81,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":982482,
         "fec_committee_id":"C00172841",
         "committee":"/committees/C00172841.json",
         "committee_name":"UNITED EGG ASSOCIATION EGGPAC",
         "form_type":"F99",
         "report_title":"MISCELLANEOUS DOCUMENT",
         "date_filed":"2015-01-07",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00172841/982482/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":982431,
         "fec_committee_id":"C00172841",
         "committee":"/committees/C00172841.json",
         "committee_name":"UNITED EGG ASSOCIATION EGGPAC",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2015-01-07",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00172841/982431/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      }
   ]
}
```

This endpoint retrieves information about FEC reports filed electronically by a committee.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/filings/search`

### Query Parameters

Parameter | Description
--------- | -----------
query | The committee name or partial name. Spaces are permitted.

## Get Electronic Filings by Date

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/filings/2016/01/10.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "date":"2016-01-10",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "offset":null,
   "results":[
      {
         "filing_id":1038725,
         "fec_committee_id":"C00494799",
         "committee":"/committees/C00494799.json",
         "committee_name":"MY AMERICA INC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00494799/1038725/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":50.0,
         "cash_on_hand":17.96,
         "disbursements_total":36.0,
         "receipts_total":50.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038724,
         "fec_committee_id":"C00572941",
         "committee":"/committees/C00572941.json",
         "committee_name":"KELLIPAC",
         "form_type":"F3",
         "report_title":"DEC MONTHLY",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2016-11-01",
         "date_coverage_to":"2016-11-30",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00572941/1038724/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"M12",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":0.0,
         "cash_on_hand":2505.0,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038723,
         "fec_committee_id":"C00320168",
         "committee":"/committees/C00320168.json",
         "committee_name":"A SECURE AMERICA PAC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00320168/1038723/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"N",
         "contributions_total":0.0,
         "cash_on_hand":154.17,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038722,
         "fec_committee_id":"C00569392",
         "committee":"/committees/C00569392.json",
         "committee_name":"JUDICAL FAIRNESS PROJECT; THE",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00569392/1038722/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":957.0,
         "cash_on_hand":2745.93,
         "disbursements_total":275.5,
         "receipts_total":957.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038721,
         "fec_committee_id":"C00449686",
         "committee":"/committees/C00449686.json",
         "committee_name":"REPUBLICAN PARTY OF MADERA COUNTY (FED.)",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-12-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00449686/1038721/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Y",
         "contributions_total":0.0,
         "cash_on_hand":7951.06,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038720,
         "fec_committee_id":"C00507939",
         "committee":"/committees/C00507939.json",
         "committee_name":"JUSTICE PARTY",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00507939/1038720/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Y",
         "contributions_total":630.0,
         "cash_on_hand":99.22,
         "disbursements_total":921.56,
         "receipts_total":630.09,
         "loans_total":"0.0",
         "debts_total":"1500.0"
      },
      {
         "filing_id":1038719,
         "fec_committee_id":"C00574129",
         "committee":"/committees/C00574129.json",
         "committee_name":"HANS 2016 LLC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00574129/1038719/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":10585.0,
         "cash_on_hand":36082.18,
         "disbursements_total":5438.66,
         "receipts_total":11058.24,
         "loans_total":"473.24",
         "debts_total":"8452.18"
      },
      {
         "filing_id":1038718,
         "fec_committee_id":"C00562173",
         "committee":"/committees/C00562173.json",
         "committee_name":"LGBT DEMOCRATS OF VIRGINIA",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00562173/1038718/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"N",
         "contributions_total":0.0,
         "cash_on_hand":2638.46,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038717,
         "fec_committee_id":"C00421453",
         "committee":"/committees/C00421453.json",
         "committee_name":"FRIENDS OF STEPHEN HARRISON",
         "form_type":"F3",
         "report_title":"TERMINATION REPORT",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00421453/1038717/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"TER",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":0.0,
         "cash_on_hand":0.0,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038716,
         "fec_committee_id":"C00575373",
         "committee":"/committees/C00575373.json",
         "committee_name":"KEEP THE PROMISE I",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-10",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00575373/1038716/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":44898.66,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1038715,
         "fec_committee_id":"C00385187",
         "committee":"/committees/C00385187.json",
         "committee_name":"YUMA COUNTY REPUBLICIAN CENTRAL COMMITTEE",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00385187/1038715/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Y",
         "contributions_total":0.0,
         "cash_on_hand":1711.3,
         "disbursements_total":860.3,
         "receipts_total":500.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038714,
         "fec_committee_id":"C00461210",
         "committee":"/committees/C00461210.json",
         "committee_name":"OHIO VETERANS UNITED PAC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00461210/1038714/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"N",
         "contributions_total":0.0,
         "cash_on_hand":1752.76,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038713,
         "fec_committee_id":"C00589226",
         "committee":"/committees/C00589226.json",
         "committee_name":"PROGRESS N@",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-10",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00589226/1038713/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00589226//",
         "committee_type":"N",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1038712,
         "fec_committee_id":"C00467779",
         "committee":"/committees/C00467779.json",
         "committee_name":"RILEY FOR CONGRESS",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00467779/1038712/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":0.0,
         "cash_on_hand":0.0,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"19173.12"
      },
      {
         "filing_id":1038711,
         "fec_committee_id":"C00017194",
         "committee":"/committees/C00017194.json",
         "committee_name":"INTERNATIONAL UNION OF OPERATING ENGINEERS LO 825 POLITICAL ACTION AND EDUCATION COMMITTEE",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00017194/1038711/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":60527.3,
         "cash_on_hand":230472.18,
         "disbursements_total":434.01,
         "receipts_total":60527.3,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038708,
         "fec_committee_id":"C00571877",
         "committee":"/committees/C00571877.json",
         "committee_name":"JIM WHITE FOR CONGRESS",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00571877/1038708/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":118.42,
         "cash_on_hand":3080.52,
         "disbursements_total":0.0,
         "receipts_total":118.42,
         "loans_total":"0.0",
         "debts_total":"5000.0"
      },
      {
         "filing_id":1038707,
         "fec_committee_id":"C00435529",
         "committee":"/committees/C00435529.json",
         "committee_name":"FOOTLIK FOR CONGRESS",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00435529/1038707/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":0.0,
         "cash_on_hand":737.72,
         "disbursements_total":381.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038706,
         "fec_committee_id":"C00540989",
         "committee":"/committees/C00540989.json",
         "committee_name":"COMMUNITY PAC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00540989/1038706/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"N",
         "contributions_total":0.0,
         "cash_on_hand":50.0,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038705,
         "fec_committee_id":"C00390658",
         "committee":"/committees/C00390658.json",
         "committee_name":"TRUEMAJORITYACTIONPAC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00390658/1038705/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":0.0,
         "cash_on_hand":721.28,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"893.54"
      },
      {
         "filing_id":1038704,
         "fec_committee_id":"C00571372",
         "committee":"/committees/C00571372.json",
         "committee_name":"RIGHT TO RISE USA",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-10",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00571372/1038704/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":98949.66,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1038703,
         "fec_committee_id":"C00571372",
         "committee":"/committees/C00571372.json",
         "committee_name":"RIGHT TO RISE USA",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-10",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00571372/1038703/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":3400.0,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1038702,
         "fec_committee_id":"C00562629",
         "committee":"/committees/C00562629.json",
         "committee_name":"SCHERTZING FOR CONGRESS",
         "form_type":"F3",
         "report_title":"TERMINATION REPORT",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00562629/1038702/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"TER",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":0.0,
         "cash_on_hand":0.0,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038701,
         "fec_committee_id":"C00394577",
         "committee":"/committees/C00394577.json",
         "committee_name":"LETICIA HINOJOSA FOR CONGRESS",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00394577/1038701/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":0.0,
         "cash_on_hand":284.28,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"60000.0"
      },
      {
         "filing_id":1038700,
         "fec_committee_id":"C00508036",
         "committee":"/committees/C00508036.json",
         "committee_name":"RAMIRO GARZA FOR CONGRESS",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00508036/1038700/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":0.0,
         "cash_on_hand":179.14,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"75200.0"
      },
      {
         "filing_id":1038699,
         "fec_committee_id":"C00559062",
         "committee":"/committees/C00559062.json",
         "committee_name":"HEADRICK FOR CONGRESS",
         "form_type":"F99",
         "report_title":"MISCELLANEOUS DOCUMENT",
         "date_filed":"2016-01-10",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00559062/1038699/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1038698,
         "fec_committee_id":"C00559062",
         "committee":"/committees/C00559062.json",
         "committee_name":"HEADRICK FOR CONGRESS",
         "form_type":"F99",
         "report_title":"MISCELLANEOUS DOCUMENT",
         "date_filed":"2016-01-10",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00559062/1038698/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1038697,
         "fec_committee_id":"C00546812",
         "committee":"/committees/C00546812.json",
         "committee_name":"IKAIKA FOR HAWAII",
         "form_type":"F3",
         "report_title":"TERMINATION REPORT",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00546812/1038697/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"TER",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":0.0,
         "cash_on_hand":0.0,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038696,
         "fec_committee_id":"C00559062",
         "committee":"/committees/C00559062.json",
         "committee_name":"HEADRICK FOR CONGRESS",
         "form_type":"F99",
         "report_title":"MISCELLANEOUS DOCUMENT",
         "date_filed":"2016-01-10",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00559062/1038696/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":null,
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1038695,
         "fec_committee_id":"C00559062",
         "committee":"/committees/C00559062.json",
         "committee_name":"HEADRICK FOR CONGRESS",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-04",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00559062/1038695/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":0.0,
         "cash_on_hand":0.0,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038694,
         "fec_committee_id":"C00570887",
         "committee":"/committees/C00570887.json",
         "committee_name":"TO REPUBLICANS OWNING THIS TOWN IN EVERY RACE PAC TROTTER PAC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00570887/1038694/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"N",
         "contributions_total":5000.0,
         "cash_on_hand":5120.0,
         "disbursements_total":88.0,
         "receipts_total":5000.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038693,
         "fec_committee_id":"C00389122",
         "committee":"/committees/C00389122.json",
         "committee_name":"RED PAC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-12-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00389122/1038693/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":0.0,
         "cash_on_hand":3234.69,
         "disbursements_total":538.25,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038691,
         "fec_committee_id":"C00550509",
         "committee":"/committees/C00550509.json",
         "committee_name":"DETROITERS VOTE",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00550509/1038691/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":300.0,
         "cash_on_hand":287.0,
         "disbursements_total":188.0,
         "receipts_total":300.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038690,
         "fec_committee_id":"C00584946",
         "committee":"/committees/C00584946.json",
         "committee_name":"DEMAREE FOR CONGRESS",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-10",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00584946/1038690/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00584946//",
         "committee_type":"H",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1038689,
         "fec_committee_id":"C00574616",
         "committee":"/committees/C00574616.json",
         "committee_name":"PHIL PAVLOV FOR CONGRESS",
         "form_type":"F3",
         "report_title":"OCT QUARTERLY",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-09-30",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00574616/1038689/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":"Q3",
         "original_filing":1029285,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00574616/1029285/",
         "committee_type":"H",
         "contributions_total":57348.64,
         "cash_on_hand":126152.56,
         "disbursements_total":31798.83,
         "receipts_total":57348.64,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038688,
         "fec_committee_id":"C00469973",
         "committee":"/committees/C00469973.json",
         "committee_name":"BRIAN ROONEY FOR CONGRESS",
         "form_type":"F3",
         "report_title":"OCT QUARTERLY",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-09-30",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00469973/1038688/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"Q3",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":0.0,
         "cash_on_hand":0.0,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"250000.0"
      },
      {
         "filing_id":1038687,
         "fec_committee_id":"C00544007",
         "committee":"/committees/C00544007.json",
         "committee_name":"FRIENDS OF ROY CHO INC.",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00544007/1038687/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":200.0,
         "cash_on_hand":520.82,
         "disbursements_total":950.45,
         "receipts_total":200.0,
         "loans_total":"0.0",
         "debts_total":"43164.85"
      },
      {
         "filing_id":1038686,
         "fec_committee_id":"C00470997",
         "committee":"/committees/C00470997.json",
         "committee_name":"MICHEL FAULKNER FOR CONGRESS",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00470997/1038686/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":0.0,
         "cash_on_hand":232.13,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"10900.0"
      },
      {
         "filing_id":1038684,
         "fec_committee_id":"C00469973",
         "committee":"/committees/C00469973.json",
         "committee_name":"BRIAN ROONEY FOR CONGRESS",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00469973/1038684/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":0.0,
         "cash_on_hand":0.0,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"250000.0"
      },
      {
         "filing_id":1038683,
         "fec_committee_id":"C00269241",
         "committee":"/committees/C00269241.json",
         "committee_name":"REPUBLICAN LIBERTY CAUCUS POLITICAL ACTION COMMITTEE",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00269241/1038683/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":69.5,
         "cash_on_hand":3426.65,
         "disbursements_total":3.22,
         "receipts_total":69.5,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038682,
         "fec_committee_id":"C00414888",
         "committee":"/committees/C00414888.json",
         "committee_name":"DECLARATION ALLIANCE PAC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00414888/1038682/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Q",
         "contributions_total":1714.0,
         "cash_on_hand":153.55,
         "disbursements_total":1647.81,
         "receipts_total":1714.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038681,
         "fec_committee_id":"C00375626",
         "committee":"/committees/C00375626.json",
         "committee_name":"CARDEN FOR CONGRESS",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00375626/1038681/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"H",
         "contributions_total":0.0,
         "cash_on_hand":0.0,
         "disbursements_total":0.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"137502.69"
      },
      {
         "filing_id":1038680,
         "fec_committee_id":"C00557355",
         "committee":"/committees/C00557355.json",
         "committee_name":"OCEAN MAJORITY PAC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00557355/1038680/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":0.0,
         "cash_on_hand":83.0,
         "disbursements_total":35.0,
         "receipts_total":0.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038679,
         "fec_committee_id":"C00400945",
         "committee":"/committees/C00400945.json",
         "committee_name":"SOUTH CAROLINA LIBERTARIAN PARTY SCLP",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-10-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00400945/1038679/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"Y",
         "contributions_total":0.0,
         "cash_on_hand":2353.42,
         "disbursements_total":0.0,
         "receipts_total":0.06,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1038678,
         "fec_committee_id":"C00484535",
         "committee":"/committees/C00484535.json",
         "committee_name":"VOTESANE PAC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-10",
         "date_coverage_from":"2015-12-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00484535/1038678/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"YE",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"N",
         "contributions_total":1000.0,
         "cash_on_hand":13285.28,
         "disbursements_total":1000.0,
         "receipts_total":1000.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      }
   ]
}
```

This endpoint retrieves information about FEC reports filed electronically on a specific date.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/filings/{year}/{month}/{day}`

### Query Parameters

Parameter | Description
--------- | -----------
year | The four-digit year from 2001-2016
month | The two-digit month from 01-12
day | The two-digit day from 01-31

## Get Electronic Filing Form Types

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/filings/types.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "results":[
      {
         "id":"F1",
         "name":"STATEMENT OF ORGANIZATION"
      },
      {
         "id":"F10",
         "name":"24-HOUR NOTICE OF EXPENDITURES FROM CANDIDATE PERSONAL FUNDS"
      },
      {
         "id":"F11",
         "name":"24-HOUR NOTICE OF EXPENDITURES FROM OPPONENT'S PERSONAL FUNDS"
      },
      {
         "id":"F12",
         "name":"24-HOUR NOTICE OF SUSPENSION OF INDIVIDUAL CONTRIBUTION LIMITS"
      },
      {
         "id":"F13",
         "name":"90 DAY POST PRESIDENTIAL INAUGURATION REPORT"
      },
      {
         "id":"F1M",
         "name":"NOTIFICATION OF MULTICANDIDATE STATUS"
      },
      {
         "id":"F2",
         "name":"STATEMENT OF CANDIDACY"
      },
      {
         "id":"F24",
         "name":"24 HOUR NOTICE"
      },
      {
         "id":"F3",
         "name":"REPORT OF RECEIPTS AND DISBURSEMENTS"
      },
      {
         "id":"F3L",
         "name":"REPORT OF CONTRIBUTIONS BUNDLED BY LOBBYIST"
      },
      {
         "id":"F4",
         "name":"REPORT OF RECEIPTS AND DISBURSEMENTS - CONVENTION"
      },
      {
         "id":"F5",
         "name":"REPORT OF INDEPENDENT EXPENDITURES MADE"
      },
      {
         "id":"F6",
         "name":"48-HOUR NOTICE OF CONTRIBUTIONS/LOANS RECEIVED"
      },
      {
         "id":"F7",
         "name":"REPORT OF COMMUNICATION COSTS"
      },
      {
         "id":"F8",
         "name":"DEBT SETTLEMENT PLAN"
      },
      {
         "id":"F9",
         "name":"24-HOUR NOTICE OF DISBURSEMENT FOR ELECTIONEERING COMMUNICATIONS"
      },
      {
         "id":"F99",
         "name":"MISCELLANEOUS SUBMISSION"
      }
   ]
}
```

This endpoint retrieves a list of available form types for FEC electronic filings.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/filings/types`

## Get Electronic Filings By Type

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/filings/types/F24.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "date":null,
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "offset":null,
   "results":[
      {
         "filing_id":1040806,
         "fec_committee_id":"C00575373",
         "committee":"/committees/C00575373.json",
         "committee_name":"KEEP THE PROMISE I",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00575373/1040806/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":183616.58,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040805,
         "fec_committee_id":"C00575373",
         "committee":"/committees/C00575373.json",
         "committee_name":"KEEP THE PROMISE I",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00575373/1040805/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":694005.46,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040788,
         "fec_committee_id":"C00581868",
         "committee":"/committees/C00581868.json",
         "committee_name":"NEW DAY FOR AMERICA",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00581868/1040788/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":372000.0,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040772,
         "fec_committee_id":"C00541292",
         "committee":"/committees/C00541292.json",
         "committee_name":"CONSERVATIVE SOLUTIONS PAC",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00541292/1040772/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":1197922.68,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040771,
         "fec_committee_id":"C00541292",
         "committee":"/committees/C00541292.json",
         "committee_name":"CONSERVATIVE SOLUTIONS PAC",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00541292/1040771/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":624420.02,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040769,
         "fec_committee_id":"C00541292",
         "committee":"/committees/C00541292.json",
         "committee_name":"CONSERVATIVE SOLUTIONS PAC",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00541292/1040769/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":1500367.44,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040726,
         "fec_committee_id":"C00489815",
         "committee":"/committees/C00489815.json",
         "committee_name":"NEA ADVOCACY FUND",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00489815/1040726/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":11894.84,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040703,
         "fec_committee_id":"C00573055",
         "committee":"/committees/C00573055.json",
         "committee_name":"AMERICA LEADS",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00573055/1040703/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":935534.52,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040693,
         "fec_committee_id":"C00575423",
         "committee":"/committees/C00575423.json",
         "committee_name":"KEEP THE PROMISE III",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00575423/1040693/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":1891.98,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040678,
         "fec_committee_id":"C00544569",
         "committee":"/committees/C00544569.json",
         "committee_name":"PURPLE PAC INC",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00544569/1040678/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":269853.0,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040666,
         "fec_committee_id":"C00532572",
         "committee":"/committees/C00532572.json",
         "committee_name":"AMERICA'S LIBERTY PAC",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00532572/1040666/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":53428.57,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040665,
         "fec_committee_id":"C00532572",
         "committee":"/committees/C00532572.json",
         "committee_name":"AMERICA'S LIBERTY PAC",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00532572/1040665/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":450000.0,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040528,
         "fec_committee_id":"C00571372",
         "committee":"/committees/C00571372.json",
         "committee_name":"RIGHT TO RISE USA",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-18",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00571372/1040528/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":24508.52,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040527,
         "fec_committee_id":"C00571372",
         "committee":"/committees/C00571372.json",
         "committee_name":"RIGHT TO RISE USA",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-18",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00571372/1040527/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":950.0,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040526,
         "fec_committee_id":"C00571372",
         "committee":"/committees/C00571372.json",
         "committee_name":"RIGHT TO RISE USA",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-18",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00571372/1040526/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":"24H",
         "original_filing":1040331,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00571372/1040331/",
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":303.34,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040512,
         "fec_committee_id":"C00575415",
         "committee":"/committees/C00575415.json",
         "committee_name":"KEEP THE PROMISE PAC",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-18",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00575415/1040512/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":755675.35,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040510,
         "fec_committee_id":"C00573055",
         "committee":"/committees/C00573055.json",
         "committee_name":"AMERICA LEADS",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-18",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00573055/1040510/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":2144.99,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040487,
         "fec_committee_id":"C00575373",
         "committee":"/committees/C00575373.json",
         "committee_name":"KEEP THE PROMISE I",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-18",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00575373/1040487/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":5965.0,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040486,
         "fec_committee_id":"C00575373",
         "committee":"/committees/C00575373.json",
         "committee_name":"KEEP THE PROMISE I",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-18",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00575373/1040486/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":3000.0,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040479,
         "fec_committee_id":"C00569905",
         "committee":"/committees/C00569905.json",
         "committee_name":"2016 COMMITTEE; THE",
         "form_type":"F24",
         "report_title":"24 HOUR NOTICE",
         "date_filed":"2016-01-18",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00569905/1040479/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":null,
         "report_period":"24H",
         "original_filing":null,
         "original_uri":null,
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":1000.0,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      }
   ]
}
```

This endpoint retrieves the most recent electronic filings by form type.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/filings/types/{form-type-id}`

### Query Parameters

Parameter | Description
--------- | -----------
form-type-id | `F` + integer. To get form type IDs, use an electronic filing form types request.

## Get Summary for a Specific Presidential Electronic Filing

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/filings/1029629.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "results":[
      {
         "filing_id":1029629,
         "fec_form_type":"F3PN",
         "report":"Q3",
         "primary_general":"P",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-09-30",
         "cash_on_hand_beginning":"2582819.11",
         "cash_on_hand_close":"1651309.13",
         "total_receipts_period":"1052657.62",
         "total_disbursements_period":"1984167.6",
         "total_receipts_cycle":"4762210.55",
         "total_disbursements_cycle":"3110901.42",
         "total_debts_owed":"0.0",
         "individual_contributions_period":"835899.34",
         "party_contributions_period":"0.0",
         "pac_contributions_period":"16750.0",
         "candidate_contributions_period":"0.0",
         "total_contributions_period":"852649.34",
         "individual_contributions_cycle":"2983650.49",
         "party_contributions_cycle":"0.0",
         "pac_contributions_cycle":"53550.0",
         "candidate_contributions_cycle":"0.0",
         "total_contributions_cycle":"3037200.49",
         "federal_funds_period":"0.0",
         "transfers_in_period":"200000.0",
         "candidate_loans_period":"0.0",
         "other_loans_period":"0.0",
         "total_loans_period":"0.0",
         "federal_funds_cycle":"0.0",
         "transfers_in_cycle":"1725000.0",
         "candidate_loans_cycle":"0.0",
         "other_loans_cycle":"0.0",
         "total_loans_cycle":"0.0",
         "operating_offsets_period":"0.0",
         "fundraising_offsets_period":"0.0",
         "legal_offsets_period":"0.0",
         "total_offsets_period":"0.0",
         "operating_offsets_cycle":"0.0",
         "fundraising_offsets_cycle":"0.0",
         "legal_offsets_cycle":"0.0",
         "total_offsets_cycle":"0.0",
         "operating_expenditures_period":"1979167.6",
         "transfers_out_period":"0.0",
         "fundraising_expenses_period":"0.0",
         "legal_expenses_period":"0.0",
         "operating_expenditures_cycle":"3094501.42",
         "transfers_out_cycle":"0.0",
         "fundraising_expenses_cycle":"0.0",
         "legal_expenses_cycle":"0.0",
         "candidate_loan_repayments_period":"0.0",
         "other_loan_repayments_period":"0.0",
         "total_loan_repayments_period":"0.0",
         "candidate_loan_repayments_cycle":"0.0",
         "other_loan_repayments_cycle":"0.0",
         "total_loan_repayments_cycle":"0.0",
         "individual_refunds_period":"5000.0",
         "party_refunds_period":"0.0",
         "pac_refunds_period":"0.0",
         "total_refunds_period":"5000.0",
         "individual_refunds_cycle":"16400.0",
         "party_refunds_cycle":"0.0",
         "pac_refunds_cycle":"0.0",
         "total_refunds_cycle":"16400.0",
         "other_disbursements_period":"0.0",
         "other_disbursements_cycle":"0.0",
         "liquidate_period":"0.0",
         "net_individual_contributions":"830899.34",
         "net_party_contributions":"0.0",
         "net_pac_contributions":null,
         "net_candidate_contributions":"0.0",
         "net_transfers_in":"200000.0",
         "net_total_contributions":"847649.34",
         "net_operating_expenses":"1979167.6",
         "net_fundraising_expenses":"0.0",
         "net_legal_expenses":"0.0",
         "net_disbursements":"1984167.6",
         "contributions_less_than_200":"76618.58",
         "num_contributions_less_than_200":418,
         "contributions_200_499":"36243.2",
         "num_contributions_200_499":146,
         "contributions_500_1499":"220785.26",
         "num_contributions_500_1499":262,
         "net_primary":"1465281.43",
         "net_general":"186027.7",
         "flag_most_current_report":null,
         "flag_valid_report":null,
         "cycle":2016,
         "created_at":null,
         "updated_at":null,
         "refunds_less_than_200":null,
         "refunds_200_499":null,
         "refunds_500_1499":null,
         "num_refunds_less_than_200":null,
         "num_refunds_200_499":null,
         "num_refunds_500_1499":null,
         "committee_uri":"/committees/C00578757.json",
         "candidate_uri":"/candidates/P60007697.json",
         "num_refunds_2500":null,
         "refunds_2500":null,
         "num_refunds_1500_2499":null,
         "refunds_1500_2499":null,
         "num_contributions_1500_2499":58,
         "contributions_1500_2499":"119651.0",
         "num_contributions_2500":138,
         "contributions_2500":"372600.0"
      }
   ]
}
```

This endpoint retrieves summary figures from a specific FEC electronic filing (Form 3 filings by presidential candidates only).

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/filings/{filing_id}`

### Query Parameters

Parameter | Description
--------- | -----------
filing_id | Integer representing the ID of a Form 3 electronic filing.

## Get Recent Amendments

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/filings/amendments.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "offset":null,
   "results":[
      {
         "filing_id":1040839,
         "fec_committee_id":"C00235036",
         "committee":"/committees/C00235036.json",
         "committee_name":"ZURICH HOLDING COMPANY OF AMERICA COMMITTEE FOR GOOD GOVERNMENT (Z-PAC)",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-20",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00235036/1040839/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00235036//",
         "committee_type":"Q",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040834,
         "fec_committee_id":"C00603787",
         "committee":"/committees/C00603787.json",
         "committee_name":"Steve Isakson for Congress",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-20",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00603787/1040834/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00603787//",
         "committee_type":"A",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040812,
         "fec_committee_id":"C00490136",
         "committee":"/committees/C00490136.json",
         "committee_name":"THE LINCOLN CLUB OF ORANGE COUNTY FEDERAL IE COMMITTEE",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00490136/1040812/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00490136//",
         "committee_type":"O",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040807,
         "fec_committee_id":"C00454819",
         "committee":"/committees/C00454819.json",
         "committee_name":"MAF FREEDOM PAC - MOVE AMERICA FORWARD FREEDOM PAC - MAF PAC",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00454819/1040807/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00454819//",
         "committee_type":"Q",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040801,
         "fec_committee_id":"C00589440",
         "committee":"/committees/C00589440.json",
         "committee_name":"SHEPHERD FOR CONGRESS",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00589440/1040801/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00589440//",
         "committee_type":"H",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040799,
         "fec_committee_id":"C00589440",
         "committee":"/committees/C00589440.json",
         "committee_name":"SHEPHERD FOR CONGRESS",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00589440/1040799/",
         "amended":true,
         "amended_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00589440/1040801/",
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00589440//",
         "committee_type":"H",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040797,
         "fec_committee_id":"C00224691",
         "committee":"/committees/C00224691.json",
         "committee_name":"ROHRABACHER FOR CONGRESS",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00224691/1040797/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00224691//",
         "committee_type":"H",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040792,
         "fec_committee_id":"C00318766",
         "committee":"/committees/C00318766.json",
         "committee_name":"CALIFORNIA INDEPENDENT PETROLEUM ASSOCIATION FEDERAL PAC",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00318766/1040792/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00318766//",
         "committee_type":"Q",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040789,
         "fec_committee_id":"C00491605",
         "committee":"/committees/C00491605.json",
         "committee_name":"STAR PARKER PAC",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00491605/1040789/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00491605//",
         "committee_type":"Q",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040786,
         "fec_committee_id":"C00552430",
         "committee":"/committees/C00552430.json",
         "committee_name":"GORELL FOR CONGRESS",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00552430/1040786/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00552430//",
         "committee_type":"H",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040784,
         "fec_committee_id":"C00136853",
         "committee":"/committees/C00136853.json",
         "committee_name":"CONTRA COSTA REPUBLICAN PARTY (FED)",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00136853/1040784/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00136853//",
         "committee_type":"Y",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040783,
         "fec_committee_id":"C00461251",
         "committee":"/committees/C00461251.json",
         "committee_name":"MAC PAC",
         "form_type":"F3",
         "report_title":"YEAR-END",
         "date_filed":"2016-01-19",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-12-31",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00461251/1040783/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":"YE",
         "original_filing":1040778,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00461251/1040778/",
         "committee_type":"N",
         "contributions_total":2000.0,
         "cash_on_hand":5590.74,
         "disbursements_total":7042.5,
         "receipts_total":2000.0,
         "loans_total":"0.0",
         "debts_total":"0.0"
      },
      {
         "filing_id":1040777,
         "fec_committee_id":"C00570168",
         "committee":"/committees/C00570168.json",
         "committee_name":"NEWPORT BEACH WOMEN'S DEMOCRATIC CLUB FEDERAL PAC",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00570168/1040777/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00570168//",
         "committee_type":"N",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040776,
         "fec_committee_id":"C00570168",
         "committee":"/committees/C00570168.json",
         "committee_name":"NEWPORT BEACH WOMEN'S DEMOCRATIC CLUB FEDERAL PAC",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00570168/1040776/",
         "amended":true,
         "amended_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00570168/1040777/",
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00570168//",
         "committee_type":"N",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040774,
         "fec_committee_id":"C00500074",
         "committee":"/committees/C00500074.json",
         "committee_name":"FRIENDS OF GARY DELONG",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00500074/1040774/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00500074//",
         "committee_type":"H",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040767,
         "fec_committee_id":"C00509554",
         "committee":"/committees/C00509554.json",
         "committee_name":"BRAD MITZELFELT FOR U.S. CONGRESS",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00509554/1040767/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00509554//",
         "committee_type":"H",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040763,
         "fec_committee_id":"C00129627",
         "committee":"/committees/C00129627.json",
         "committee_name":"PIPEFITTERS POLITICAL ACTION COMMITTEE",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00129627/1040763/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00129627//",
         "committee_type":"Q",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040760,
         "fec_committee_id":"C00387555",
         "committee":"/committees/C00387555.json",
         "committee_name":"COUNCIL FOR A LIVABLE WORLD CANDIDATE FUND",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00387555/1040760/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00387555//",
         "committee_type":"Q",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040757,
         "fec_committee_id":"C00515064",
         "committee":"/committees/C00515064.json",
         "committee_name":"YOUNG DEMOCRATS OF AMERICA POLITICAL ACTION COMMITTEE",
         "form_type":"F1",
         "report_title":"STATEMENT OF ORGANIZATION",
         "date_filed":"2016-01-19",
         "date_coverage_from":null,
         "date_coverage_to":null,
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00515064/1040757/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":null,
         "original_filing":null,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00515064//",
         "committee_type":"V",
         "contributions_total":null,
         "cash_on_hand":null,
         "disbursements_total":null,
         "receipts_total":null,
         "loans_total":null,
         "debts_total":null
      },
      {
         "filing_id":1040740,
         "fec_committee_id":"C00437913",
         "committee":"/committees/C00437913.json",
         "committee_name":"OLSON FOR CONGRESS COMMITTEE",
         "form_type":"F3",
         "report_title":"OCT QUARTERLY",
         "date_filed":"2016-01-19",
         "date_coverage_from":"2015-07-01",
         "date_coverage_to":"2015-09-30",
         "fec_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00437913/1040740/",
         "amended":false,
         "amended_uri":null,
         "is_amendment":true,
         "report_period":"Q3",
         "original_filing":1027374,
         "original_uri":"http://docquery.fec.gov/cgi-bin/dcdev/forms/C00437913/1027374/",
         "committee_type":"H",
         "contributions_total":205407.0,
         "cash_on_hand":415445.23,
         "disbursements_total":132687.63,
         "receipts_total":205687.05,
         "loans_total":"0.0",
         "debts_total":"0.0"
      }
   ]
}
```

This endpoint retrieves the most recent filings that are amendments of earlier filings.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/filings/amendments`

# Electioneering Communications

## Get Recent Electioneering Communications

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/electioneering_communications.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2008/",
   "cycle":2008,
   "results":[
      {
         "cycle":2008,
         "fec_committee_id":"C30000871",
         "committee_name":"AMERICAN LEADERSHIP PROJECT",
         "payee_organization":"Buying Time",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"2715 M Street NW",
         "payee_address_2":null,
         "payee_city":"Washington",
         "payee_state":"DC",
         "payee_zip":"20007",
         "expenditure_date":"2008-03-03",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"144000.0",
         "entity_type":"ORG",
         "transaction_id":"F93.000002",
         "filing_id":333398,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-04-15",
         "unique_id":"76c13c12bc4aab4126feb88282b1811f4074b8af",
         "electioneering_communication_candidates":[
            {
               "id":85,
               "electioneering_communication_id":77,
               "fec_candidate_id":"3084",
               "candidate_name":"Clinton, Hillary",
               "back_reference_tran_id_number":"F93.000002",
               "filing_id":333398,
               "candidate_state":"OH",
               "candidate_district":null,
               "transaction_id":"F94.000005",
               "amended_from":null,
               "created_at":"2012-02-13T21:13:48.000-05:00",
               "updated_at":"2013-05-23T13:44:47.000-04:00"
            },
            {
               "id":86,
               "electioneering_communication_id":77,
               "fec_candidate_id":"18641",
               "candidate_name":"Obama, Barack",
               "back_reference_tran_id_number":"F93.000002",
               "filing_id":333398,
               "candidate_state":"OH",
               "candidate_district":null,
               "transaction_id":"F94.000006",
               "amended_from":null,
               "created_at":"2012-02-13T21:13:48.000-05:00",
               "updated_at":"2013-05-23T13:44:47.000-04:00"
            }
         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"A. Gutierrez & Associates Inc",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"715 South Upper Broadway",
         "payee_address_2":"Suite 702",
         "payee_city":"Corpus Christi",
         "payee_state":"TX",
         "payee_zip":"78401",
         "expenditure_date":"2008-02-29",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"285485.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4161",
         "filing_id":325365,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-03-01",
         "unique_id":"d977aa7f5b8203415d3b9927e252f26b7ee6abdd",
         "electioneering_communication_candidates":[
            {
               "id":82,
               "electioneering_communication_id":75,
               "fec_candidate_id":"18641",
               "candidate_name":"Obama, Barack",
               "back_reference_tran_id_number":"F93.4161",
               "filing_id":325365,
               "candidate_state":"TX",
               "candidate_district":null,
               "transaction_id":"F94.4105",
               "amended_from":null,
               "created_at":"2012-02-13T21:13:44.000-05:00",
               "updated_at":"2013-05-23T13:44:46.000-04:00"
            }
         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000871",
         "committee_name":"AMERICAN LEADERSHIP PROJECT",
         "payee_organization":"The Davis Group",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"3601 S Congress Avenue",
         "payee_address_2":null,
         "payee_city":"Austin",
         "payee_state":"TX",
         "payee_zip":"78701",
         "expenditure_date":"2008-02-29",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"513354.0",
         "entity_type":"ORG",
         "transaction_id":"F93.000001",
         "filing_id":333398,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-04-15",
         "unique_id":"0d09ff5a801cf01291a4148c7d324e556d54b16e",
         "electioneering_communication_candidates":[
            {
               "id":83,
               "electioneering_communication_id":76,
               "fec_candidate_id":"3084",
               "candidate_name":"Clinton, Hillary",
               "back_reference_tran_id_number":"F93.000001",
               "filing_id":333398,
               "candidate_state":"TX",
               "candidate_district":null,
               "transaction_id":"F94.000002",
               "amended_from":null,
               "created_at":"2012-02-13T21:13:48.000-05:00",
               "updated_at":"2013-05-23T13:44:47.000-04:00"
            },
            {
               "id":84,
               "electioneering_communication_id":76,
               "fec_candidate_id":"18641",
               "candidate_name":"Obama, Barack",
               "back_reference_tran_id_number":"F93.000001",
               "filing_id":333398,
               "candidate_state":"TX",
               "candidate_district":null,
               "transaction_id":"F94.000003",
               "amended_from":null,
               "created_at":"2012-02-13T21:13:48.000-05:00",
               "updated_at":"2013-05-23T13:44:47.000-04:00"
            }
         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"Persuasion FX/shjMedia",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"800 Fourth St. SW Suite S121",
         "payee_address_2":null,
         "payee_city":"Washington",
         "payee_state":"DC",
         "payee_zip":"20002",
         "expenditure_date":"2008-02-03",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"40600.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4159",
         "filing_id":321236,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-04",
         "unique_id":"0274cd42fc23d5618a26a4ed690206bac5d3f08b",
         "electioneering_communication_candidates":[
            {
               "id":81,
               "electioneering_communication_id":74,
               "fec_candidate_id":"18641",
               "candidate_name":"Obama, Barack",
               "back_reference_tran_id_number":"F93.4159",
               "filing_id":321236,
               "candidate_state":"NY",
               "candidate_district":null,
               "transaction_id":"F94.4105",
               "amended_from":null,
               "created_at":"2012-02-13T21:13:44.000-05:00",
               "updated_at":"2013-05-23T13:44:46.000-04:00"
            }
         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"KPWR/Emmis Communications",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"2600 West Olive Ave. 8th Floor",
         "payee_address_2":null,
         "payee_city":"Burbank",
         "payee_state":"CA",
         "payee_zip":"91505",
         "expenditure_date":"2008-02-02",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"10000.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4156",
         "filing_id":320906,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-03",
         "unique_id":"3a800247838d3e394b8d1e5917cbfada7b523037",
         "electioneering_communication_candidates":[

         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"KXOL",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"10281 West Pico Blvd",
         "payee_address_2":null,
         "payee_city":"Los Angeles",
         "payee_state":"CA",
         "payee_zip":"90064",
         "expenditure_date":"2008-02-02",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"10000.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4157",
         "filing_id":320906,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-03",
         "unique_id":"366c5279fa53948a04021943d42658abf5a0bf35",
         "electioneering_communication_candidates":[

         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"Univision Radio",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"750 Battery Street Suite 200",
         "payee_address_2":null,
         "payee_city":"San Francisco",
         "payee_state":"CA",
         "payee_zip":"94111",
         "expenditure_date":"2008-02-02",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"5220.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4154",
         "filing_id":320906,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-03",
         "unique_id":"c55d04edbdc4c470989af5435ff694b71014894c",
         "electioneering_communication_candidates":[

         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"KJLH",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"161 North La Brea",
         "payee_address_2":null,
         "payee_city":"Inglewood",
         "payee_state":"CA",
         "payee_zip":"90301",
         "expenditure_date":"2008-01-31",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"15000.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4139",
         "filing_id":320864,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-01",
         "unique_id":"4b12ad10c98aeee71167464babf1df871614cf01",
         "electioneering_communication_candidates":[

         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"93.5 KDAY / Magic Broadcasting",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"5055 Wilshire Blvd. Suite 720",
         "payee_address_2":null,
         "payee_city":"Los Angeles",
         "payee_state":"CA",
         "payee_zip":"90036",
         "expenditure_date":"2008-01-31",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"10000.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4137",
         "filing_id":320864,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-01",
         "unique_id":"3fc48a89c9357bcbe9da242b2800305559ec978c",
         "electioneering_communication_candidates":[

         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"Univision Radio Los Angeles",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"655 North Central Avenue, Suite 25",
         "payee_address_2":null,
         "payee_city":"Glendale",
         "payee_state":"CA",
         "payee_zip":"91203",
         "expenditure_date":"2008-02-02",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"20150.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4153",
         "filing_id":320906,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-03",
         "unique_id":"3737ec557b51bb6df4f2d2eab7611a67df4756c7",
         "electioneering_communication_candidates":[
            {
               "id":74,
               "electioneering_communication_id":73,
               "fec_candidate_id":"18641",
               "candidate_name":"Obama, Barack",
               "back_reference_tran_id_number":"F93.4152",
               "filing_id":320906,
               "candidate_state":"CA",
               "candidate_district":null,
               "transaction_id":"F94.4105",
               "amended_from":null,
               "created_at":"2012-02-13T21:13:44.000-05:00",
               "updated_at":"2013-05-23T13:44:46.000-04:00"
            }
         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"A. Gutierrez & Associates Inc",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"715 South Upper Broadway",
         "payee_address_2":"Suite 702",
         "payee_city":"Corpus Christi",
         "payee_state":"TX",
         "payee_zip":"78401",
         "expenditure_date":"2008-02-02",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"55000.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4152",
         "filing_id":320906,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-03",
         "unique_id":"5870302551678b41ffee59424f68ac74b745930e",
         "electioneering_communication_candidates":[

         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"Univision Radio Los Angeles",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"655 North Central Avenue, Suite 25",
         "payee_address_2":null,
         "payee_city":"Glendale",
         "payee_state":"CA",
         "payee_zip":"91203",
         "expenditure_date":"2008-01-26",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"44700.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4112",
         "filing_id":317344,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-01-27",
         "unique_id":"0ccf978f36ad0af08ab46227951aecc556ec09a9",
         "electioneering_communication_candidates":[
            {
               "id":67,
               "electioneering_communication_id":60,
               "fec_candidate_id":"18641",
               "candidate_name":"Obama, Barack",
               "back_reference_tran_id_number":"F93.4112",
               "filing_id":317344,
               "candidate_state":"CA",
               "candidate_district":null,
               "transaction_id":"F94.4105",
               "amended_from":null,
               "created_at":"2012-02-13T21:13:40.000-05:00",
               "updated_at":"2013-05-23T13:44:44.000-04:00"
            }
         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"KHHT FM Clear Channel Broadcasting",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"3400 W. Olive St., Suite 1650",
         "payee_address_2":null,
         "payee_city":"Burbank",
         "payee_state":"CA",
         "payee_zip":"91505",
         "expenditure_date":"2008-02-02",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"10000.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4155",
         "filing_id":320906,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-03",
         "unique_id":"9d0add7153cc016216932c252180e69927367638",
         "electioneering_communication_candidates":[

         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"Levitical Network",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"4859 W. Slauson Ave., #370",
         "payee_address_2":null,
         "payee_city":"Los Angeles",
         "payee_state":"CA",
         "payee_zip":"90056",
         "expenditure_date":"2008-01-31",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"1500.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4141",
         "filing_id":320864,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-01",
         "unique_id":"708c12f80cda48a96214c60c21b901c91dd8dc3b",
         "electioneering_communication_candidates":[
            {
               "id":68,
               "electioneering_communication_id":66,
               "fec_candidate_id":"18641",
               "candidate_name":"Obama, Barack",
               "back_reference_tran_id_number":"F93.4137",
               "filing_id":320864,
               "candidate_state":"CA",
               "candidate_district":null,
               "transaction_id":"F94.4105",
               "amended_from":null,
               "created_at":"2012-02-13T21:13:42.000-05:00",
               "updated_at":"2013-05-23T13:44:45.000-04:00"
            }
         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"KBMB FM",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"1436 Auburn Blvd.",
         "payee_address_2":null,
         "payee_city":"Sacramento",
         "payee_state":"CA",
         "payee_zip":"95815",
         "expenditure_date":"2008-02-02",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"10000.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4150",
         "filing_id":320906,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-03",
         "unique_id":"45904fa29d8d6d1aff9721cf218ec62e0369f817",
         "electioneering_communication_candidates":[

         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"KRBV-FM",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"5900 Wilshire Blvd Suite 1900",
         "payee_address_2":null,
         "payee_city":"Los Angeles",
         "payee_state":"CA",
         "payee_zip":"90036",
         "expenditure_date":"2008-01-31",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"8500.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4140",
         "filing_id":320864,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-01",
         "unique_id":"08eaf73d5015c1fa1b1309a3c1e176913b7bbaa2",
         "electioneering_communication_candidates":[

         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"Clear Channel Broadcasting",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"340 Townsend St.",
         "payee_address_2":null,
         "payee_city":"San Francisco",
         "payee_state":"CA",
         "payee_zip":"94107",
         "expenditure_date":"2008-01-31",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"15000.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4138",
         "filing_id":320864,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-01",
         "unique_id":"385b9eca08a30e6d6d7077e6666eea700141345f",
         "electioneering_communication_candidates":[

         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000822",
         "committee_name":"PowerPac.org",
         "payee_organization":"Finest City Broadcasting",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"9660 Granite Ridge Drive Suite200",
         "payee_address_2":null,
         "payee_city":"San Diego",
         "payee_state":"CA",
         "payee_zip":"92123",
         "expenditure_date":"2008-01-31",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"10000.0",
         "entity_type":"ORG",
         "transaction_id":"F93.4143",
         "filing_id":320864,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-02-01",
         "unique_id":"81e4cfbc636eeefeaa7d1968787ec7308d33ab7b",
         "electioneering_communication_candidates":[

         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000871",
         "committee_name":"AMERICAN LEADERSHIP PROJECT",
         "payee_organization":"Point of View Productions",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"400 Gordon Drive",
         "payee_address_2":null,
         "payee_city":"Exton",
         "payee_state":"PA",
         "payee_zip":"19341",
         "expenditure_date":"2008-03-03",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"33000.0",
         "entity_type":"ORG",
         "transaction_id":"F93.000004",
         "filing_id":333398,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-04-15",
         "unique_id":"807756c87911cff021ea3de483dd0e79ac5343cd",
         "electioneering_communication_candidates":[
            {
               "id":88,
               "electioneering_communication_id":79,
               "fec_candidate_id":"3084",
               "candidate_name":"Clinton, Hillary",
               "back_reference_tran_id_number":"F93.000004",
               "filing_id":333398,
               "candidate_state":"OH",
               "candidate_district":null,
               "transaction_id":"F94.000010",
               "amended_from":null,
               "created_at":"2012-02-13T21:13:48.000-05:00",
               "updated_at":"2013-05-23T13:44:47.000-04:00"
            },
            {
               "id":89,
               "electioneering_communication_id":79,
               "fec_candidate_id":"18641",
               "candidate_name":"Obama, Barack",
               "back_reference_tran_id_number":"F93.000004",
               "filing_id":333398,
               "candidate_state":"OH",
               "candidate_district":null,
               "transaction_id":"F94.000011",
               "amended_from":null,
               "created_at":"2012-02-13T21:13:48.000-05:00",
               "updated_at":"2013-05-23T13:44:47.000-04:00"
            }
         ]
      },
      {
         "cycle":2008,
         "fec_committee_id":"C30000871",
         "committee_name":"AMERICAN LEADERSHIP PROJECT",
         "payee_organization":"See Change LLC",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"8609 West Knoll Drive",
         "payee_address_2":null,
         "payee_city":"West Hollywood",
         "payee_state":"CA",
         "payee_zip":"90089",
         "expenditure_date":"2008-02-22",
         "communication_date":null,
         "purpose":null,
         "election_code":"P2008",
         "amount":"30941.0",
         "entity_type":"ORG",
         "transaction_id":"F93.000003",
         "filing_id":333398,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2008-04-15",
         "unique_id":"6dd27002f7d5c53e95b284aad63d6f9a604b0077",
         "electioneering_communication_candidates":[
            {
               "id":87,
               "electioneering_communication_id":78,
               "fec_candidate_id":"3084",
               "candidate_name":"Clinton, Hillary",
               "back_reference_tran_id_number":"F93.000003",
               "filing_id":333398,
               "candidate_state":"OH",
               "candidate_district":null,
               "transaction_id":"F94.000008",
               "amended_from":null,
               "created_at":"2012-02-13T21:13:48.000-05:00",
               "updated_at":"2013-05-23T13:44:47.000-04:00"
            }
         ]
      }
   ]
}
```

This endpoint retrieves the 20 most recent broadcast advertisements that identify one or more federal candidates (and have aired 30 days before a primary election and 60 days before the general election).

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/electioneering_communications`

## Get Electioneering Communications by Specific Committee

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/committees/C30002034/electioneering_communications.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "results":[
      {
         "cycle":2012,
         "fec_committee_id":"C30002034",
         "committee_name":"United Against Illegal Guns Support Fund",
         "payee_organization":"Devine Mulvey, Inc.",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"2141 Wisconsin Avenue, NW",
         "payee_address_2":"Suite H",
         "payee_city":"Washington",
         "payee_state":"DC",
         "payee_zip":null,
         "expenditure_date":"2012-10-01",
         "communication_date":"2012-10-01",
         "purpose":"Media Production - 48,000",
         "election_code":"G2012",
         "amount":"28160.0",
         "entity_type":"ORG",
         "transaction_id":"F93.000001",
         "filing_id":812623,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2012-10-02",
         "unique_id":"72125f7c0025e7f7f2f70a5c6c149377d52069c9",
         "electioneering_communication_candidates":[
            {
               "id":1099,
               "electioneering_communication_id":996,
               "fec_candidate_id":"P80003338",
               "candidate_name":"Obama, Barack",
               "back_reference_tran_id_number":"F93.000001",
               "filing_id":812623,
               "candidate_state":null,
               "candidate_district":null,
               "transaction_id":"F94.000002",
               "amended_from":null,
               "created_at":"2012-10-02T21:15:32.000-04:00",
               "updated_at":"2012-10-02T21:15:32.000-04:00"
            },
            {
               "id":1100,
               "electioneering_communication_id":996,
               "fec_candidate_id":"P80003353",
               "candidate_name":"Romney, Mitt",
               "back_reference_tran_id_number":"F93.000001",
               "filing_id":812623,
               "candidate_state":null,
               "candidate_district":null,
               "transaction_id":"F94.000003",
               "amended_from":null,
               "created_at":"2012-10-02T21:15:32.000-04:00",
               "updated_at":"2012-10-02T21:15:32.000-04:00"
            }
         ]
      },
      {
         "cycle":2012,
         "fec_committee_id":"C30002034",
         "committee_name":"United Against Illegal Guns Support Fund",
         "payee_organization":"Buying Time, LLC",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"650 Massachusetts Avenue, NW",
         "payee_address_2":"Suite 210",
         "payee_city":"Washington",
         "payee_state":"DC",
         "payee_zip":null,
         "expenditure_date":"2012-09-25",
         "communication_date":"2012-10-01",
         "purpose":"Media Buy - 48,000",
         "election_code":"G2012",
         "amount":"150500.0",
         "entity_type":"ORG",
         "transaction_id":"F93.000002",
         "filing_id":812623,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2012-10-02",
         "unique_id":"251ca3aba6a564f6fec7ddb50cee40595bb6d37b",
         "electioneering_communication_candidates":[
            {
               "id":1101,
               "electioneering_communication_id":997,
               "fec_candidate_id":"P80003338",
               "candidate_name":"Obama, Barack",
               "back_reference_tran_id_number":"F93.000002",
               "filing_id":812623,
               "candidate_state":null,
               "candidate_district":null,
               "transaction_id":"F94.000005",
               "amended_from":null,
               "created_at":"2012-10-02T21:15:32.000-04:00",
               "updated_at":"2012-10-02T21:15:32.000-04:00"
            },
            {
               "id":1102,
               "electioneering_communication_id":997,
               "fec_candidate_id":"P80003353",
               "candidate_name":"Romney, Mitt",
               "back_reference_tran_id_number":"F93.000002",
               "filing_id":812623,
               "candidate_state":null,
               "candidate_district":null,
               "transaction_id":"F94.000006",
               "amended_from":null,
               "created_at":"2012-10-02T21:15:32.000-04:00",
               "updated_at":"2012-10-02T21:15:32.000-04:00"
            }
         ]
      }
   ]
}
```

This endpoint retrieves the most recent broadcast advertisements by a specific committee that identify one or more federal candidates (and have aired 30 days before a primary election and 60 days before the general election).

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/committees/{fec-id}/electioneering_communications`

### Query Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a committee. To find a candidate's official FEC ID, use a candidate search request or the [FEC web site](http://www.fec.gov).

## Get Electioneering Communications by Date

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/electioneering_communications/2012/10/01.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "results":[
      {
         "cycle":2012,
         "fec_committee_id":"C30002034",
         "committee_name":"United Against Illegal Guns Support Fund",
         "payee_organization":"Devine Mulvey, Inc.",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"2141 Wisconsin Avenue, NW",
         "payee_address_2":"Suite H",
         "payee_city":"Washington",
         "payee_state":"DC",
         "payee_zip":null,
         "expenditure_date":"2012-10-01",
         "communication_date":"2012-10-01",
         "purpose":"Media Production - 48,000",
         "election_code":"G2012",
         "amount":"28160.0",
         "entity_type":"ORG",
         "transaction_id":"F93.000001",
         "filing_id":812623,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2012-10-02",
         "unique_id":"72125f7c0025e7f7f2f70a5c6c149377d52069c9",
         "electioneering_communication_candidates":[
            {
               "id":1099,
               "electioneering_communication_id":996,
               "fec_candidate_id":"P80003338",
               "candidate_name":"Obama, Barack",
               "back_reference_tran_id_number":"F93.000001",
               "filing_id":812623,
               "candidate_state":null,
               "candidate_district":null,
               "transaction_id":"F94.000002",
               "amended_from":null,
               "created_at":"2012-10-02T21:15:32.000-04:00",
               "updated_at":"2012-10-02T21:15:32.000-04:00"
            },
            {
               "id":1100,
               "electioneering_communication_id":996,
               "fec_candidate_id":"P80003353",
               "candidate_name":"Romney, Mitt",
               "back_reference_tran_id_number":"F93.000001",
               "filing_id":812623,
               "candidate_state":null,
               "candidate_district":null,
               "transaction_id":"F94.000003",
               "amended_from":null,
               "created_at":"2012-10-02T21:15:32.000-04:00",
               "updated_at":"2012-10-02T21:15:32.000-04:00"
            }
         ]
      },
      {
         "cycle":2012,
         "fec_committee_id":"C30002034",
         "committee_name":"United Against Illegal Guns Support Fund",
         "payee_organization":"Buying Time, LLC",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"650 Massachusetts Avenue, NW",
         "payee_address_2":"Suite 210",
         "payee_city":"Washington",
         "payee_state":"DC",
         "payee_zip":null,
         "expenditure_date":"2012-09-25",
         "communication_date":"2012-10-01",
         "purpose":"Media Buy - 48,000",
         "election_code":"G2012",
         "amount":"150500.0",
         "entity_type":"ORG",
         "transaction_id":"F93.000002",
         "filing_id":812623,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2012-10-02",
         "unique_id":"251ca3aba6a564f6fec7ddb50cee40595bb6d37b",
         "electioneering_communication_candidates":[
            {
               "id":1101,
               "electioneering_communication_id":997,
               "fec_candidate_id":"P80003338",
               "candidate_name":"Obama, Barack",
               "back_reference_tran_id_number":"F93.000002",
               "filing_id":812623,
               "candidate_state":null,
               "candidate_district":null,
               "transaction_id":"F94.000005",
               "amended_from":null,
               "created_at":"2012-10-02T21:15:32.000-04:00",
               "updated_at":"2012-10-02T21:15:32.000-04:00"
            },
            {
               "id":1102,
               "electioneering_communication_id":997,
               "fec_candidate_id":"P80003353",
               "candidate_name":"Romney, Mitt",
               "back_reference_tran_id_number":"F93.000002",
               "filing_id":812623,
               "candidate_state":null,
               "candidate_district":null,
               "transaction_id":"F94.000006",
               "amended_from":null,
               "created_at":"2012-10-02T21:15:32.000-04:00",
               "updated_at":"2012-10-02T21:15:32.000-04:00"
            }
         ]
      },
      {
         "cycle":2012,
         "fec_committee_id":"C30001051",
         "committee_name":"AMERICANS FOR PROSPERITY",
         "payee_organization":"TARGET ENTERPRISE, LLC",
         "payee_last_name":null,
         "payee_first_name":null,
         "payee_middle_name":null,
         "payee_suffix":null,
         "payee_address_1":"15260 VENTURA BLVD.",
         "payee_address_2":"SUITE 1240",
         "payee_city":"SHERMAN OAKS",
         "payee_state":"CA",
         "payee_zip":null,
         "expenditure_date":"2012-09-20",
         "communication_date":"2012-10-01",
         "purpose":"PLACEMENT OF RADIO ADVERTISEMENT (\"OWE IT\")",
         "election_code":"G2012",
         "amount":"332044.0",
         "entity_type":"ORG",
         "transaction_id":"F93.000002",
         "filing_id":812707,
         "back_reference_tran_id_number":null,
         "back_reference_sched_name":null,
         "amended_from":null,
         "filed_date":"2012-10-03",
         "unique_id":"a83b0a556407c047fc7e600aae1e1fce4fe85d18",
         "electioneering_communication_candidates":[
            {
               "id":1104,
               "electioneering_communication_id":999,
               "fec_candidate_id":"P80003338",
               "candidate_name":"OBAMA, BARACK",
               "back_reference_tran_id_number":"F93.000002",
               "filing_id":812707,
               "candidate_state":null,
               "candidate_district":null,
               "transaction_id":"F94.000004",
               "amended_from":null,
               "created_at":"2012-10-03T11:30:37.000-04:00",
               "updated_at":"2012-10-03T11:30:37.000-04:00"
            }
         ]
      }
   ]
}
```

This endpoint retrieves all broadcast advertisements that identify one or more federal candidates (and have aired 30 days before a primary election and 60 days before the general election) from a specific date.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/electioneering_communications/{year}/{month}/{day}`

### Query Parameters

Parameter | Description
--------- | -----------
year | The four-digit year from 2008-2016
month | The two-digit month from 01-12
day | The two-digit day from 01-31

# Independent Expenditures

## Get Recent Independent Expenditures

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/independent_expenditures.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
	"status": "OK",
	"copyright": "Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
	"base_uri": "https://api.propublica.org/campaign-finance/v1/2016/",
	"cycle": 2016,
	"results": [{
		"fec_committee": "/committees/C00608489.json",
		"fec_committee_id": "C00608489",
		"fec_committee_name": "GREAT AMERICA PAC",
		"fec_candidate": "/candidates/P80001571.json",
		"fec_candidate_id": "P80001571",
		"candidate_name": "DONALD TRUMP",
		"amount": 20100.0,
		"office": "President",
		"state": null,
		"district": null,
		"date": "2016-07-19",
		"purpose": "RADIO ADVERTISING",
		"payee": "RAPID RESPONSE TELEVISION LLC",
		"date_received": "2016-07-19",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE24.85887",
		"unique_id": "96d173b94ee0fd697d81c2c9771679431e0f21fc",
		"filing_id": 1088603,
		"amended_from": null,
		"dissemination_date": "2016-08-01",
		"form_type": "F24"
	}, {
		"fec_committee": "/committees/C00608489.json",
		"fec_committee_id": "C00608489",
		"fec_committee_name": "GREAT AMERICA PAC",
		"fec_candidate": "/candidates/P80001571.json",
		"fec_candidate_id": "P80001571",
		"candidate_name": "DONALD TRUMP",
		"amount": 44591.84,
		"office": "President",
		"state": null,
		"district": null,
		"date": "2016-07-18",
		"purpose": "TELEVISION ADVERTISING",
		"payee": "RAPID RESPONSE TELEVISION LLC",
		"date_received": "2016-07-18",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE24.85813",
		"unique_id": "2287bbb30ac6118fff40e41f289724c8369483c4",
		"filing_id": 1088151,
		"amended_from": 1080594,
		"dissemination_date": "2016-07-28",
		"form_type": "F24"
	}, {
		"fec_committee": "/committees/C00608489.json",
		"fec_committee_id": "C00608489",
		"fec_committee_name": "GREAT AMERICA PAC",
		"fec_candidate": "/candidates/P80001571.json",
		"fec_candidate_id": "P80001571",
		"candidate_name": "DONALD TRUMP",
		"amount": 44591.84,
		"office": "President",
		"state": null,
		"district": null,
		"date": "2016-07-19",
		"purpose": "TELEVISION ADVERTISING",
		"payee": "RAPID RESPONSE TELEVISION LLC",
		"date_received": "2016-07-19",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE24.85888",
		"unique_id": "e024ea77eb25e4c961eda97d780b569f77ece8e9",
		"filing_id": 1088603,
		"amended_from": null,
		"dissemination_date": "2016-07-28",
		"form_type": "F24"
	}, {
		"fec_committee": "/committees/C00608489.json",
		"fec_committee_id": "C00608489",
		"fec_committee_name": "GREAT AMERICA PAC",
		"fec_candidate": "/candidates/P80001571.json",
		"fec_candidate_id": "P80001571",
		"candidate_name": "DONALD TRUMP",
		"amount": 15000.0,
		"office": "President",
		"state": null,
		"district": null,
		"date": "2016-07-18",
		"purpose": "ESTIMATED CREATIVE SERVICES FEES",
		"payee": "BRILLIANT COMMUNICATIONS",
		"date_received": "2016-07-18",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE24.85808",
		"unique_id": "53e7ac1ae198532ec56638012c7d374e21e30263",
		"filing_id": 1088137,
		"amended_from": null,
		"dissemination_date": "2016-07-26",
		"form_type": "F24"
	}, {
		"fec_committee": "/committees/C00608489.json",
		"fec_committee_id": "C00608489",
		"fec_committee_name": "GREAT AMERICA PAC",
		"fec_candidate": "/candidates/P80001571.json",
		"fec_candidate_id": "P80001571",
		"candidate_name": "DONALD TRUMP",
		"amount": 2000.0,
		"office": "President",
		"state": null,
		"district": null,
		"date": "2016-07-18",
		"purpose": "ESTIMATED MAIL AND PRODUCTION FEES",
		"payee": "BRILLIANT COMMUNICATIONS",
		"date_received": "2016-07-18",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE24.85810",
		"unique_id": "a3ab1c73de835b88cadf9d9c305ce474e4a8f63a",
		"filing_id": 1088137,
		"amended_from": null,
		"dissemination_date": "2016-07-26",
		"form_type": "F24"
	}, {
		"fec_committee": "/committees/C00489856.json",
		"fec_committee_id": "C00489856",
		"fec_committee_name": "ESAFUND",
		"fec_candidate": "/candidates/H6KS01146.json",
		"fec_candidate_id": "H6KS01146",
		"candidate_name": "Timothy A. Huelskamp",
		"amount": 8271.32,
		"office": "House",
		"state": "KS",
		"district": 1,
		"date": null,
		"purpose": "telephone calls",
		"payee": "RedPrint Strategy",
		"date_received": "2016-07-22",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "O",
		"transaction_id": "SE.6681",
		"unique_id": "e96a5e95f8639d5838bbb11dba842988b1afab00",
		"filing_id": 1090323,
		"amended_from": null,
		"dissemination_date": "2016-07-21",
		"form_type": "F24"
	}]
}
```

This endpoint retrieves the 200 most recent independent expenditures and optionally can be offset by multiples of 20.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/independent_expenditures`

## Get Independent Expenditures by Date

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/independent_expenditures/2015/01/31.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
	"status": "OK",
	"copyright": "Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
	"base_uri": "https://api.propublica.org/campaign-finance/v1/2016/",
	"cycle": 2016,
	"date": "2015-01-31",
	"results": [{
		"fec_committee": "/committees/C00448696.json",
		"fec_committee_id": "C00448696",
		"fec_committee_name": "SENATE CONSERVATIVES FUND",
		"fec_candidate": "/candidates/S0UT00165.json",
		"fec_candidate_id": "S0UT00165",
		"candidate_name": "Mike Lee",
		"amount": 28.75,
		"office": "Senate",
		"state": "UT",
		"district": null,
		"date": "2015-01-31",
		"purpose": "IE-Lee-Online Processing",
		"payee": "Senate Conservatives Fund",
		"date_received": "2015-07-31",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "E4E72CD473970430A95C",
		"unique_id": "a9ef7c98a2085d2438ca55d06c94e8dbfdfe26e6",
		"filing_id": 1019911,
		"amended_from": null,
		"dissemination_date": "2015-01-31",
		"form_type": "F3"
	}]
}
```

This endpoint retrieves all independent expenditures on a specific date (the date of activity, not the date filed with the FEC).

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/independent_expenditures/{year}/{month}/{day}`

### Query Parameters

Parameter | Description
--------- | -----------
year | The four-digit year from 2008-2016
month | The two-digit month from 01-12
day | The two-digit day from 01-31

## Get Independent Expenditures by Specific Committee

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/committees/C00575423/independent_expenditures.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
	"status": "OK",
	"copyright": "Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
	"base_uri": "https://api.propublica.org/campaign-finance/v1/2016/",
	"cycle": 2016,
	"fec_committee": "/committees/C00575423.json",
	"total_amount": 4611668.79,
	"offset": null,
	"results": [{
		"fec_committee_name": "KEEP THE PROMISE III",
		"fec_candidate": "/candidates/P60006111.json",
		"fec_candidate_id": "P60006111",
		"candidate_name": "RAFAEL 'TED' CRUZ",
		"amount": 11750.0,
		"office": "President",
		"state": "WI",
		"district": null,
		"date": "2016-04-05",
		"purpose": "ROBO CALLS - SEE RED METRICS 3-31-16",
		"payee": "VICTORY PHONES",
		"date_received": "2016-07-25",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE.12128",
		"unique_id": "0ca009e290728da60061dbdd95a99b99141c9405",
		"filing_id": 1090759,
		"amended_from": 1073631,
		"dissemination_date": "2016-04-05",
		"form_type": "F3"
	}, {
		"fec_committee_name": "KEEP THE PROMISE III",
		"fec_candidate": "/candidates/P60006111.json",
		"fec_candidate_id": "P60006111",
		"candidate_name": "RAFAEL 'TED' CRUZ",
		"amount": 11750.0,
		"office": "President",
		"state": "WI",
		"district": null,
		"date": "2016-04-04",
		"purpose": "ROBO CALLS - SEE RED METRICS 3-31-16",
		"payee": "VICTORY PHONES",
		"date_received": "2016-07-25",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE.12126",
		"unique_id": "5b5d0e711751bc45ee01601fa1ee1974481c3419",
		"filing_id": 1090759,
		"amended_from": 1073631,
		"dissemination_date": "2016-04-04",
		"form_type": "F3"
	}, {
		"fec_committee_name": "KEEP THE PROMISE III",
		"fec_candidate": "/candidates/P60006111.json",
		"fec_candidate_id": "P60006111",
		"candidate_name": "RAFAEL 'TED' CRUZ",
		"amount": 8512.65,
		"office": "President",
		"state": "WI",
		"district": null,
		"date": "2016-04-04",
		"purpose": "DIGITAL MEDIA PRODUCTION/PLACEMENT - SEE RED METRICS 3-31-16",
		"payee": "FACEBOOK",
		"date_received": "2016-07-25",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE.12125",
		"unique_id": "2bf4b082cf45a8858b882b7aae54fe497dc41cdc",
		"filing_id": 1090759,
		"amended_from": 1073631,
		"dissemination_date": "2016-04-04",
		"form_type": "F3"
	}, {
		"fec_committee_name": "KEEP THE PROMISE III",
		"fec_candidate": "/candidates/P60006111.json",
		"fec_candidate_id": "P60006111",
		"candidate_name": "RAFAEL 'TED' CRUZ",
		"amount": 5000.0,
		"office": "President",
		"state": "WI",
		"district": null,
		"date": "2016-04-03",
		"purpose": "DIGITAL MEDIA PRODUCTION/PLACEMENT - SEE RED METRICS 3-31-16",
		"payee": "FACEBOOK",
		"date_received": "2016-07-25",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE.12124",
		"unique_id": "cc5335bfdeac6d2319b3453c88ead40fea1ee279",
		"filing_id": 1090759,
		"amended_from": 1073631,
		"dissemination_date": "2016-04-03",
		"form_type": "F3"
	}, {
		"fec_committee_name": "KEEP THE PROMISE III",
		"fec_candidate": "/candidates/P60006111.json",
		"fec_candidate_id": "P60006111",
		"candidate_name": "RAFAEL 'TED' CRUZ",
		"amount": 5605.15,
		"office": "President",
		"state": "IA",
		"district": null,
		"date": "2016-02-01",
		"purpose": "DIGITAL MEDIA PRODUCTION/PLACEMENT - SEE RED METRICS 2-24-16",
		"payee": "YOUTUBE",
		"date_received": "2016-07-05",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE.10566",
		"unique_id": "978fd65516bd9898f8e7ceb95703f45076a93dd7",
		"filing_id": 1081039,
		"amended_from": 1056897,
		"dissemination_date": "2016-02-01",
		"form_type": "F3"
	}]
}
```

This endpoint retrieves the 20 most recent independent expenditures by a given committee.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/committees/{fec-id}/independent_expenditures`

### Query Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a committee. To find a candidate's official FEC ID, use a candidate search request or the [FEC web site](http://www.fec.gov).

## Get Independent Expenditures that Support or Oppose a Specific Candidate

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/candidates/P00003392/independent_expenditures.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
	"status": "OK",
	"copyright": "Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
	"base_uri": "https://api.propublica.org/campaign-finance/v1/2016/",
	"cycle": 2016,
	"fec_candidate": "/candidates/P00003392.json",
	"support_total": 13834304.1399999,
	"oppose_total": 12763328.56,
	"offset": null,
	"results": [{
		"fec_committee": "/committees/C90016106.json",
		"fec_committee_name": "AFL-CIO COMMITTEE ON POLITICAL EDUCATION TREASURY FUND",
		"candidate_name": "HILLARY RODHAM CLINTON",
		"amount": 361.56,
		"office": "President",
		"state": null,
		"district": null,
		"date": "2016-06-17",
		"purpose": "Printing and Shipping - Campaign Signs",
		"payee": "AFL-CIO Support Services",
		"date_received": "2016-07-15",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "F57.4103",
		"unique_id": "1ef578fe2e60fda963715812f84fff1a66f89122",
		"filing_id": 1085825,
		"amended_from": null,
		"dissemination_date": "2016-06-17",
		"form_type": "F5"
	}, {
		"fec_committee": "/committees/C90016106.json",
		"fec_committee_name": "AFL-CIO COMMITTEE ON POLITICAL EDUCATION TREASURY FUND",
		"candidate_name": "HILLARY RODHAM CLINTON",
		"amount": 706.66,
		"office": "President",
		"state": null,
		"district": null,
		"date": "2016-06-20",
		"purpose": "Printing and Shipping - Campaign Signs",
		"payee": "AFL-CIO Support Services",
		"date_received": "2016-07-15",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "F57.4104",
		"unique_id": "f6331bb017bab2351679af3d2ef8273ce936480e",
		"filing_id": 1085825,
		"amended_from": null,
		"dissemination_date": "2016-06-20",
		"form_type": "F5"
	}, {
		"fec_committee": "/committees/C90016106.json",
		"fec_committee_name": "AFL-CIO COMMITTEE ON POLITICAL EDUCATION TREASURY FUND",
		"candidate_name": "HILLARY RODHAM CLINTON",
		"amount": 3136.35,
		"office": "President",
		"state": null,
		"district": null,
		"date": "2016-06-21",
		"purpose": "Printing and Shipping - Campaign Signs",
		"payee": "AFL-CIO Support Services",
		"date_received": "2016-07-15",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "F57.4105",
		"unique_id": "465fc4b5c44e598ddd99b238380cd4e7805aafa9",
		"filing_id": 1085825,
		"amended_from": null,
		"dissemination_date": "2016-06-21",
		"form_type": "F5"
	}, {
		"fec_committee": "/committees/C90016106.json",
		"fec_committee_name": "AFL-CIO COMMITTEE ON POLITICAL EDUCATION TREASURY FUND",
		"candidate_name": "HILLARY RODHAM CLINTON",
		"amount": 709.46,
		"office": "President",
		"state": null,
		"district": null,
		"date": "2016-06-22",
		"purpose": "Printing and Shipping - Campaign Signs",
		"payee": "AFL-CIO Support Services",
		"date_received": "2016-07-15",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "F57.4106",
		"unique_id": "e93c2a973a44c2cffe0998a3b50d6ee4ba7f94f9",
		"filing_id": 1085825,
		"amended_from": null,
		"dissemination_date": "2016-06-22",
		"form_type": "F5"
	}, {
		"fec_committee": "/committees/C00497420.json",
		"fec_committee_name": "CITIZENS UNITED SUPER PAC LLC",
		"candidate_name": "Hillary Clinton",
		"amount": 711.67,
		"office": "President",
		"state": null,
		"district": null,
		"date": "2016-04-18",
		"purpose": "Creative fee for Direct Mail opposing Hillary Clinton for Pres (mailed 3/9/16 no 48 hr rpt filed)",
		"payee": "HSP Direct",
		"date_received": "2016-07-15",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "O",
		"transaction_id": "B620011",
		"unique_id": "32fee6c089d5152c91b314a46e84c97cde8dc265",
		"filing_id": 1087326,
		"amended_from": null,
		"dissemination_date": "2016-04-18",
		"form_type": "F3"
	}]
}
```

This endpoint retrieves the 200 most recent independent expenditures in support of or opposition to a given candidate.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/candidates/{fec-id}/independent_expenditures`

### Query Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a committee. To find a candidate's official FEC ID, use a candidate search request or the [FEC web site](http://www.fec.gov).

## Get Independent Expenditures that Support or Oppose Presidential Candidates

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/president/independent_expenditures.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
	"status": "OK",
	"copyright": "Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
	"base_uri": "https://api.propublica.org/campaign-finance/v1/2016/",
	"cycle": 2016,
	"results": [{
		"fec_committee": "/committees/C00575423.json",
		"fec_committee_id": "C00575423",
		"fec_committee_name": "KEEP THE PROMISE III",
		"fec_candidate": "/candidates/P60006111.json",
		"fec_candidate_id": "P60006111",
		"candidate_name": "RAFAEL 'TED' CRUZ",
		"amount": 4770.1,
		"office": "President",
		"state": "WI",
		"district": null,
		"date": "2016-04-05",
		"purpose": "DIGITAL MEDIA PRODUCTION/PLACEMENT - SEE RED METRICS 3-31-16",
		"payee": "FACEBOOK",
		"date_received": "2016-07-25",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE.12127",
		"unique_id": "a2518555b5f18d1365fd97fb79599c843ca2657e",
		"filing_id": 1090759,
		"amended_from": 1073631,
		"dissemination_date": "2016-04-05",
		"form_type": "F3"
	}, {
		"fec_committee": "/committees/C00575423.json",
		"fec_committee_id": "C00575423",
		"fec_committee_name": "KEEP THE PROMISE III",
		"fec_candidate": "/candidates/P60006111.json",
		"fec_candidate_id": "P60006111",
		"candidate_name": "RAFAEL 'TED' CRUZ",
		"amount": 11750.0,
		"office": "President",
		"state": "WI",
		"district": null,
		"date": "2016-04-05",
		"purpose": "ROBO CALLS - SEE RED METRICS 3-31-16",
		"payee": "VICTORY PHONES",
		"date_received": "2016-07-25",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE.12128",
		"unique_id": "0ca009e290728da60061dbdd95a99b99141c9405",
		"filing_id": 1090759,
		"amended_from": 1073631,
		"dissemination_date": "2016-04-05",
		"form_type": "F3"
	}, {
		"fec_committee": "/committees/C00575423.json",
		"fec_committee_id": "C00575423",
		"fec_committee_name": "KEEP THE PROMISE III",
		"fec_candidate": "/candidates/P60006111.json",
		"fec_candidate_id": "P60006111",
		"candidate_name": "RAFAEL 'TED' CRUZ",
		"amount": 11750.0,
		"office": "President",
		"state": "WI",
		"district": null,
		"date": "2016-04-04",
		"purpose": "ROBO CALLS - SEE RED METRICS 3-31-16",
		"payee": "VICTORY PHONES",
		"date_received": "2016-07-25",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE.12126",
		"unique_id": "5b5d0e711751bc45ee01601fa1ee1974481c3419",
		"filing_id": 1090759,
		"amended_from": 1073631,
		"dissemination_date": "2016-04-04",
		"form_type": "F3"
	}, {
		"fec_committee": "/committees/C00575423.json",
		"fec_committee_id": "C00575423",
		"fec_committee_name": "KEEP THE PROMISE III",
		"fec_candidate": "/candidates/P60006111.json",
		"fec_candidate_id": "P60006111",
		"candidate_name": "RAFAEL 'TED' CRUZ",
		"amount": 8512.65,
		"office": "President",
		"state": "WI",
		"district": null,
		"date": "2016-04-04",
		"purpose": "DIGITAL MEDIA PRODUCTION/PLACEMENT - SEE RED METRICS 3-31-16",
		"payee": "FACEBOOK",
		"date_received": "2016-07-25",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE.12125",
		"unique_id": "2bf4b082cf45a8858b882b7aae54fe497dc41cdc",
		"filing_id": 1090759,
		"amended_from": 1073631,
		"dissemination_date": "2016-04-04",
		"form_type": "F3"
	}, {
		"fec_committee": "/committees/C00575423.json",
		"fec_committee_id": "C00575423",
		"fec_committee_name": "KEEP THE PROMISE III",
		"fec_candidate": "/candidates/P60006111.json",
		"fec_candidate_id": "P60006111",
		"candidate_name": "RAFAEL 'TED' CRUZ",
		"amount": 5000.0,
		"office": "President",
		"state": "WI",
		"district": null,
		"date": "2016-04-03",
		"purpose": "DIGITAL MEDIA PRODUCTION/PLACEMENT - SEE RED METRICS 3-31-16",
		"payee": "FACEBOOK",
		"date_received": "2016-07-25",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "S",
		"transaction_id": "SE.12124",
		"unique_id": "cc5335bfdeac6d2319b3453c88ead40fea1ee279",
		"filing_id": 1090759,
		"amended_from": 1073631,
		"dissemination_date": "2016-04-03",
		"form_type": "F3"
	}, {
		"fec_committee": "/committees/C90011156.json",
		"fec_committee_id": "C90011156",
		"fec_committee_name": "WORKING AMERICA",
		"fec_candidate": "/candidates/P80001571.json",
		"fec_candidate_id": "P80001571",
		"candidate_name": "Donald Trump",
		"amount": 27.93,
		"office": "President",
		"state": null,
		"district": null,
		"date": "2016-07-21",
		"purpose": "Salary and Benefits",
		"payee": "Marcus Howell",
		"date_received": "2016-07-22",
		"fec_uri": null,
		"amendment": null,
		"support_or_oppose": "O",
		"transaction_id": "VN7CZA1TP38",
		"unique_id": "f1a68bc21d3445e3fe74380866ec532af48e3d75",
		"filing_id": 1090327,
		"amended_from": null,
		"dissemination_date": "2016-07-21",
		"form_type": "F5"
	}]
}
```

This endpoint retrieves the 200 most recent independent expenditures in support of or opposition to any presidential candidate.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/president/independent_expenditures`

## Get Independent Expenditure Office Totals

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/independent_expenditures/race_totals/president.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "results":[
      {
         "state":"NH",
         "office":"President",
         "district":null,
         "amount":"43813300.1900001"
      },
      {
         "state":"IA",
         "office":"President",
         "district":null,
         "amount":"39169448.72"
      },
      {
         "state":null,
         "office":"President",
         "district":null,
         "amount":"21053952.16"
      },
      {
         "state":"SC",
         "office":"President",
         "district":null,
         "amount":"11535840.63"
      },
      {
         "state":"NH",
         "office":"President",
         "district":"0",
         "amount":"11208105.2"
      },
      {
         "state":"WA",
         "office":"President",
         "district":"5",
         "amount":"7851303.62"
      },
      {
         "state":"IA",
         "office":"President",
         "district":"0",
         "amount":"4238537.85"
      },
      {
         "state":null,
         "office":"President",
         "district":"0",
         "amount":"3491088.69"
      },
      {
         "state":"MA",
         "office":"President",
         "district":null,
         "amount":"2193662.61"
      },
      {
         "state":"NV",
         "office":"President",
         "district":null,
         "amount":"1611572.18"
      },
      {
         "state":"SC",
         "office":"President",
         "district":"0",
         "amount":"1127370.33"
      },
      {
         "state":"NV",
         "office":"President",
         "district":"0",
         "amount":"612167.11"
      },
      {
         "state":"OH",
         "office":"President",
         "district":null,
         "amount":"483992.06"
      },
      {
         "state":"FL",
         "office":"President",
         "district":"0",
         "amount":"411560.62"
      },
      {
         "state":"FL",
         "office":"President",
         "district":null,
         "amount":"265830.34"
      },
      {
         "state":"CA",
         "office":"President",
         "district":null,
         "amount":"213111.59"
      },
      {
         "state":"NY",
         "office":"President",
         "district":null,
         "amount":"170207.67"
      },
      {
         "state":"VT",
         "office":"President",
         "district":null,
         "amount":"167418.43"
      },
      {
         "state":"TX",
         "office":"President",
         "district":null,
         "amount":"141696.36"
      },
      {
         "state":"CA",
         "office":"President",
         "district":"0",
         "amount":"105363.86"
      },
      {
         "state":"VA",
         "office":"President",
         "district":"0",
         "amount":"91814.96"
      },
      {
         "state":"IL",
         "office":"President",
         "district":null,
         "amount":"89425.9"
      },
      {
         "state":"CO",
         "office":"President",
         "district":"0",
         "amount":"88029.96"
      },
      {
         "state":"GA",
         "office":"President",
         "district":null,
         "amount":"79341.72"
      },
      {
         "state":"PA",
         "office":"President",
         "district":null,
         "amount":"77411.91"
      },
      {
         "state":"VA",
         "office":"President",
         "district":null,
         "amount":"74627.83"
      },
      {
         "state":"TX",
         "office":"President",
         "district":"0",
         "amount":"73854.49"
      },
      {
         "state":"MN",
         "office":"President",
         "district":null,
         "amount":"71425.97"
      },
      {
         "state":"MI",
         "office":"President",
         "district":null,
         "amount":"65599.0"
      },
      {
         "state":"NY",
         "office":"President",
         "district":"0",
         "amount":"62426.17"
      },
      {
         "state":"NC",
         "office":"President",
         "district":null,
         "amount":"62289.71"
      },
      {
         "state":"WI",
         "office":"President",
         "district":null,
         "amount":"60247.41"
      },
      {
         "state":"OK",
         "office":"President",
         "district":"0",
         "amount":"51900.56"
      },
      {
         "state":"NJ",
         "office":"President",
         "district":null,
         "amount":"50328.32"
      },
      {
         "state":"TN",
         "office":"President",
         "district":null,
         "amount":"46622.46"
      },
      {
         "state":"PA",
         "office":"President",
         "district":"0",
         "amount":"45506.48"
      },
      {
         "state":"IL",
         "office":"President",
         "district":"0",
         "amount":"44822.82"
      },
      {
         "state":"AR",
         "office":"President",
         "district":null,
         "amount":"43495.65"
      },
      {
         "state":"AL",
         "office":"President",
         "district":null,
         "amount":"43347.75"
      },
      {
         "state":"OH",
         "office":"President",
         "district":"0",
         "amount":"41838.49"
      },
      {
         "state":"WA",
         "office":"President",
         "district":null,
         "amount":"40674.86"
      },
      {
         "state":"CO",
         "office":"President",
         "district":null,
         "amount":"39721.8"
      },
      {
         "state":"LA",
         "office":"President",
         "district":"0",
         "amount":"38787.93"
      },
      {
         "state":"AZ",
         "office":"President",
         "district":null,
         "amount":"38472.35"
      },
      {
         "state":"AL",
         "office":"President",
         "district":"0",
         "amount":"38109.78"
      },
      {
         "state":"MI",
         "office":"President",
         "district":"0",
         "amount":"37710.8"
      },
      {
         "state":"NC",
         "office":"President",
         "district":"0",
         "amount":"37023.95"
      },
      {
         "state":"GA",
         "office":"President",
         "district":"0",
         "amount":"36882.9"
      },
      {
         "state":"IN",
         "office":"President",
         "district":null,
         "amount":"36424.95"
      },
      {
         "state":"NJ",
         "office":"President",
         "district":"0",
         "amount":"35226.36"
      },
      {
         "state":"MO",
         "office":"President",
         "district":null,
         "amount":"34225.45"
      },
      {
         "state":"MD",
         "office":"President",
         "district":null,
         "amount":"33425.08"
      },
      {
         "state":"OK",
         "office":"President",
         "district":null,
         "amount":"32518.19"
      },
      {
         "state":"WA",
         "office":"President",
         "district":"0",
         "amount":"30139.95"
      },
      {
         "state":"MA",
         "office":"President",
         "district":"0",
         "amount":"29926.46"
      },
      {
         "state":"IN",
         "office":"President",
         "district":"0",
         "amount":"29072.02"
      },
      {
         "state":"TN",
         "office":"President",
         "district":"0",
         "amount":"29045.72"
      },
      {
         "state":"AZ",
         "office":"President",
         "district":"0",
         "amount":"28870.97"
      },
      {
         "state":"MO",
         "office":"President",
         "district":"0",
         "amount":"28030.74"
      },
      {
         "state":"MD",
         "office":"President",
         "district":"0",
         "amount":"27651.19"
      },
      {
         "state":"WI",
         "office":"President",
         "district":"0",
         "amount":"27339.25"
      },
      {
         "state":"LA",
         "office":"President",
         "district":null,
         "amount":"26394.81"
      },
      {
         "state":"MN",
         "office":"President",
         "district":"0",
         "amount":"26306.15"
      },
      {
         "state":"KY",
         "office":"President",
         "district":null,
         "amount":"25652.59"
      },
      {
         "state":"KY",
         "office":"President",
         "district":"0",
         "amount":"23972.25"
      },
      {
         "state":"OR",
         "office":"President",
         "district":null,
         "amount":"23448.98"
      },
      {
         "state":"OR",
         "office":"President",
         "district":"0",
         "amount":"22867.47"
      },
      {
         "state":"CT",
         "office":"President",
         "district":"0",
         "amount":"22118.54"
      },
      {
         "state":"CT",
         "office":"President",
         "district":null,
         "amount":"21736.97"
      },
      {
         "state":"MS",
         "office":"President",
         "district":"0",
         "amount":"20335.88"
      },
      {
         "state":"AR",
         "office":"President",
         "district":"0",
         "amount":"20333.4"
      },
      {
         "state":"KS",
         "office":"President",
         "district":"0",
         "amount":"20073.07"
      },
      {
         "state":"UT",
         "office":"President",
         "district":"0",
         "amount":"19390.01"
      },
      {
         "state":"NM",
         "office":"President",
         "district":"0",
         "amount":"18056.27"
      },
      {
         "state":"MS",
         "office":"President",
         "district":null,
         "amount":"17971.44"
      },
      {
         "state":"WV",
         "office":"President",
         "district":"0",
         "amount":"17876.11"
      },
      {
         "state":"NE",
         "office":"President",
         "district":"0",
         "amount":"17590.44"
      },
      {
         "state":"KS",
         "office":"President",
         "district":null,
         "amount":"17436.83"
      },
      {
         "state":"ID",
         "office":"President",
         "district":"0",
         "amount":"16857.7"
      },
      {
         "state":"AK",
         "office":"President",
         "district":null,
         "amount":"16644.95"
      },
      {
         "state":"HI",
         "office":"President",
         "district":"0",
         "amount":"16576.36"
      },
      {
         "state":"ME",
         "office":"President",
         "district":"0",
         "amount":"16539.89"
      },
      {
         "state":"UT",
         "office":"President",
         "district":null,
         "amount":"15973.46"
      },
      {
         "state":"RI",
         "office":"President",
         "district":"0",
         "amount":"15802.3"
      },
      {
         "state":"MT",
         "office":"President",
         "district":"0",
         "amount":"15620.76"
      },
      {
         "state":"DE",
         "office":"President",
         "district":"0",
         "amount":"15382.54"
      },
      {
         "state":"SD",
         "office":"President",
         "district":"0",
         "amount":"15117.84"
      },
      {
         "state":"00",
         "office":"President",
         "district":"0",
         "amount":"15043.55"
      },
      {
         "state":"AK",
         "office":"President",
         "district":"0",
         "amount":"14836.54"
      },
      {
         "state":"ND",
         "office":"President",
         "district":"0",
         "amount":"14831.67"
      },
      {
         "state":"VT",
         "office":"President",
         "district":"0",
         "amount":"14726.6"
      },
      {
         "state":"WY",
         "office":"President",
         "district":"0",
         "amount":"14508.39"
      },
      {
         "state":"NM",
         "office":"President",
         "district":null,
         "amount":"13866.14"
      },
      {
         "state":"WV",
         "office":"President",
         "district":null,
         "amount":"12775.57"
      },
      {
         "state":"NE",
         "office":"President",
         "district":null,
         "amount":"12124.11"
      },
      {
         "state":"ID",
         "office":"President",
         "district":null,
         "amount":"10624.42"
      },
      {
         "state":"HI",
         "office":"President",
         "district":null,
         "amount":"10034.9"
      },
      {
         "state":"ME",
         "office":"President",
         "district":null,
         "amount":"10009.67"
      },
      {
         "state":"RI",
         "office":"President",
         "district":null,
         "amount":"8405.56"
      },
      {
         "state":"MT",
         "office":"President",
         "district":null,
         "amount":"8015.17"
      },
      {
         "state":"DE",
         "office":"President",
         "district":null,
         "amount":"7508.42"
      },
      {
         "state":"SD",
         "office":"President",
         "district":null,
         "amount":"6949.23"
      },
      {
         "state":"ND",
         "office":"President",
         "district":null,
         "amount":"6344.79"
      },
      {
         "state":"WY",
         "office":"President",
         "district":null,
         "amount":"5662.07"
      },
      {
         "state":"AS",
         "office":"President",
         "district":null,
         "amount":"336.98"
      },
      {
         "state":"MP",
         "office":"President",
         "district":null,
         "amount":"336.98"
      },
      {
         "state":"GU",
         "office":"President",
         "district":null,
         "amount":"336.98"
      },
      {
         "state":"VI",
         "office":"President",
         "district":null,
         "amount":"336.96"
      },
      {
         "state":"PR",
         "office":"President",
         "district":null,
         "amount":"336.96"
      },
      {
         "state":"CO",
         "office":"President",
         "district":"5",
         "amount":"19.1"
      }
   ]
}
```

This endpoint retrieves the amount of money spent in independent expenditures for a given office (either House, Senate or President).

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/independent_expenditures/race_totals/{office}`

### Query Parameters

Parameter | Description
--------- | -----------
office | one of `house`, `senate` or `president`

## Get Independent Expenditure Race Totals for a Specific Committee

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/committees/C00490375/independent_expenditures/races.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "fec_committee":"/committees/C00490375.json",
   "total_amount":971008.12,
   "house_total":0.0,
   "senate_total":0.0,
   "president_total":971008.12,
   "offset":null,
   "results":[
      {
         "state":null,
         "office":"President",
         "district":0,
         "amount":663477.58
      },
      {
         "state":"IA",
         "office":"President",
         "district":0,
         "amount":181777.86
      },
      {
         "state":"NV",
         "office":"President",
         "district":0,
         "amount":92541.95
      },
      {
         "state":"SC",
         "office":"President",
         "district":0,
         "amount":23911.5
      },
      {
         "state":"NH",
         "office":"President",
         "district":0,
         "amount":9299.23
      }
   ]
}
```

This endpoint retrieves the total amounts of money that a given committee has spent on individual races (consisting of a state, office and district) during a cycle.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/committees/{fec-id}/independent_expenditures/races`

### Query Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a committee. To find a candidate's official FEC ID, use a candidate search request or the [FEC web site](http://www.fec.gov).

# Late Contributions to Candidates

## Get Recent Late Contributions

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/contributions/48hour.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "offset":null,
   "results":[
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024237,
         "transaction_id":"6DE024C3050E240A79B1",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"NRA Political Victory Fund",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"11250 Waples Mill Rd",
         "contributor_street_2":null,
         "contributor_city":"Fairfax",
         "contributor_state":"VA",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-07",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024237,
         "transaction_id":"65C903E1837EE4CC8AF4",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"THE FARM CREDIT COUNCIL POLITICAL ACTION COMMITTEE",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"50 F STREET NW",
         "contributor_street_2":"SUITE 900",
         "contributor_city":"WASHINGTON",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-07",
         "contribution_amount":"2000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024159,
         "transaction_id":"6C1DC0CA5724F4C7B86F",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"ALTRIA GROUP, INC. POLITICAL ACTION COMMITTEE (ALTRIAPAC)",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"101 CONSTITUTION AVE NW",
         "contributor_street_2":"SUITE 400W",
         "contributor_city":"WASHINGTON",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-05",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024159,
         "transaction_id":"6DFB4ABAF3C0747E39F3",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"FMC CORPORATION GOOD GOVERNMENT PROGRAM",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1050 K STREET, NW",
         "contributor_street_2":"SUITE 600",
         "contributor_city":"WASHINGTON",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-05",
         "contribution_amount":"2000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024159,
         "transaction_id":"6739A98EA4D6B4888B6D",
         "entity_type":"IND",
         "contributor_fec_id":null,
         "contributor_organization_name":null,
         "contributor_prefix":null,
         "contributor_first_name":"Daniel",
         "contributor_middle_name":null,
         "contributor_last_name":"Gallagher",
         "contributor_suffix":null,
         "contributor_street_1":"1454 Olive Rd",
         "contributor_street_2":null,
         "contributor_city":"Homewood",
         "contributor_state":"IL",
         "contributor_zip":null,
         "contributor_employer":"Information Requested",
         "contributor_occupation":"Information Requested",
         "contribution_date":"2015-09-05",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024159,
         "transaction_id":"6927FCB14CA3846CC947",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"Eye of the Tiger PAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"PO Box 2485",
         "contributor_street_2":null,
         "contributor_city":"Springfield",
         "contributor_state":"VA",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-05",
         "contribution_amount":"5000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024076,
         "transaction_id":"69E8D217D148741CFA22",
         "entity_type":"IND",
         "contributor_fec_id":null,
         "contributor_organization_name":null,
         "contributor_prefix":null,
         "contributor_first_name":"Peter",
         "contributor_middle_name":null,
         "contributor_last_name":"Fisher",
         "contributor_suffix":null,
         "contributor_street_1":"114 Glenridge Dr",
         "contributor_street_2":null,
         "contributor_city":"East Peoria",
         "contributor_state":"IL",
         "contributor_zip":null,
         "contributor_employer":"State of Illinois",
         "contributor_occupation":"Prisoner Review Board Member",
         "contribution_date":"2015-09-04",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      }
   ],
   "callback":null
}
```

During the last 20 days before a primary or general election, candidate committees must file reports of any contributions of $1,000 or more within 48 hours of receipt. This endpoint retrieves the most recent late contributions to candidates.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/contributions/48hour`

## Get Recent Late Contributions to a Specific Candidate

```shell
curl "https://api.propublica.org/campaign-finance/v1/2014/candidates/H4NY11138/48hour.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2014/",
   "cycle":2014,
   "offset":null,
   "results":[
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":972424,
         "transaction_id":"C10115420",
         "entity_type":"PAC",
         "contributor_fec_id":"C00007922",
         "contributor_organization_name":"LIUNA PAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"905 16th St NW",
         "contributor_street_2":"Fl 2",
         "contributor_city":"Washington",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":"LiUna! PAC",
         "contributor_occupation":"Legislative & Political Director",
         "contribution_date":"2014-11-03",
         "contribution_amount":"2500.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":972135,
         "transaction_id":"C10114340",
         "entity_type":"PAC",
         "contributor_fec_id":"C00007922",
         "contributor_organization_name":"LIUNA PAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"905 16th St NW",
         "contributor_street_2":"Fl 2",
         "contributor_city":"Washington",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":"LiUna! PAC",
         "contributor_occupation":"Legislative & Political Director",
         "contribution_date":"2014-11-01",
         "contribution_amount":"2500.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":972135,
         "transaction_id":"C10114334",
         "entity_type":"PAC",
         "contributor_fec_id":"C00002089",
         "contributor_organization_name":"Communication Workers of AmericaCWA-COPE",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"501 3rd St NW",
         "contributor_street_2":null,
         "contributor_city":"Washington",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-11-01",
         "contribution_amount":"1500.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":970648,
         "transaction_id":"C10111815",
         "entity_type":"PAC",
         "contributor_fec_id":"C00112888",
         "contributor_organization_name":"Lorillard Tobacco Company PAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"714 Green Valley Rd",
         "contributor_street_2":null,
         "contributor_city":"Greensboro",
         "contributor_state":"NC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-10-28",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":970648,
         "transaction_id":"C10112501",
         "entity_type":"PAC",
         "contributor_fec_id":"C00012476",
         "contributor_organization_name":"UA Political Educational Committee",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"THREE PARK PLACE",
         "contributor_street_2":null,
         "contributor_city":"ANNAPOLIS",
         "contributor_state":"MD",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-10-29",
         "contribution_amount":"1500.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":968525,
         "transaction_id":"C10106422",
         "entity_type":"PAC",
         "contributor_fec_id":"C00107300",
         "contributor_organization_name":"American Airlines PAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1101 17th St NW",
         "contributor_street_2":null,
         "contributor_city":"Washington",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":"American Airlines",
         "contributor_occupation":null,
         "contribution_date":"2014-10-23",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":968525,
         "transaction_id":"C10106591",
         "entity_type":"IND",
         "contributor_fec_id":null,
         "contributor_organization_name":null,
         "contributor_prefix":null,
         "contributor_first_name":"Michael",
         "contributor_middle_name":null,
         "contributor_last_name":"Doppelt",
         "contributor_suffix":null,
         "contributor_street_1":"235 W 70th St",
         "contributor_street_2":"Apt 3F",
         "contributor_city":"New York",
         "contributor_state":"NY",
         "contributor_zip":null,
         "contributor_employer":"Lightyear Capital",
         "contributor_occupation":"Private Equity-Fundraiser",
         "contribution_date":"2014-10-23",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":968525,
         "transaction_id":"C10107257",
         "entity_type":"PAC",
         "contributor_fec_id":"C00373423",
         "contributor_organization_name":"Bricklayers and Allied Craftworkers",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"3750 Monroe Ave",
         "contributor_street_2":"Ste 17A",
         "contributor_city":"Pittsford",
         "contributor_state":"NY",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-10-23",
         "contribution_amount":"2500.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":964070,
         "transaction_id":"C10102045",
         "entity_type":"PAC",
         "contributor_fec_id":"C00373423",
         "contributor_organization_name":"Bricklayers and Allied Craftworkers",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"3750 Monroe Ave",
         "contributor_street_2":"Ste 17A",
         "contributor_city":"Pittsford",
         "contributor_state":"NY",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-10-20",
         "contribution_amount":"2500.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":961162,
         "transaction_id":"C10099748",
         "entity_type":"PAC",
         "contributor_fec_id":"C00003251",
         "contributor_organization_name":"NEA Fund for Children & Public Education",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1201 16th St NW",
         "contributor_street_2":"Ste 420",
         "contributor_city":"Washington",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":"NEA Fund for Children and Public Educa",
         "contributor_occupation":"Chairperson",
         "contribution_date":"2014-10-17",
         "contribution_amount":"2000.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":932956,
         "transaction_id":"C9906322",
         "entity_type":"PAC",
         "contributor_fec_id":"C00027359",
         "contributor_organization_name":"Ironworkers Political Action League (IPAL)",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1750 New York Ave NW",
         "contributor_street_2":null,
         "contributor_city":"Washington",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-06-20",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":932059,
         "transaction_id":"C9902228",
         "entity_type":"PAC",
         "contributor_fec_id":"C70003108",
         "contributor_organization_name":"International Association of Fire Fighters",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1750 New York Ave NW",
         "contributor_street_2":null,
         "contributor_city":"Washington",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-06-16",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":931012,
         "transaction_id":"C9901568",
         "entity_type":"PAC",
         "contributor_fec_id":"C00010082",
         "contributor_organization_name":"National Cable & Telecom",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"25 MASSACHUSETTS AVENUE, NW #100",
         "contributor_street_2":null,
         "contributor_city":"WASHINGTON",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-06-13",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":931012,
         "transaction_id":"C9901567",
         "entity_type":"PAC",
         "contributor_fec_id":"C00009936",
         "contributor_organization_name":"AFGE Political Action Committee",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"80 F St NW",
         "contributor_street_2":"attn:Derrick Thomas, National Vice",
         "contributor_city":"Washington",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":"American Federation of Govt. Employees",
         "contributor_occupation":null,
         "contribution_date":"2014-06-13",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":931012,
         "transaction_id":"C9901474",
         "entity_type":"PAC",
         "contributor_fec_id":"C00088591",
         "contributor_organization_name":"Employees of Northrop Grumman Corp. PAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"520 S Grand Ave",
         "contributor_street_2":"Ste 700",
         "contributor_city":"Los Angeles",
         "contributor_state":"CA",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-06-13",
         "contribution_amount":"2500.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":930498,
         "transaction_id":"C9900257",
         "entity_type":"PAC",
         "contributor_fec_id":"C00003251",
         "contributor_organization_name":"The NEA Fund for Children & Public Education",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1201 16th St NW",
         "contributor_street_2":"Ste 420",
         "contributor_city":"Washington",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":"NEA Fund for Children and Public Educa",
         "contributor_occupation":"Chairperson",
         "contribution_date":"2014-06-11",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":930498,
         "transaction_id":"C9900269",
         "entity_type":"PAC",
         "contributor_fec_id":"C00068692",
         "contributor_organization_name":"Federal Express PAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"942 S Shady Grove Rd",
         "contributor_street_2":"# Memphis",
         "contributor_city":"Memphis",
         "contributor_state":"TN",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-06-11",
         "contribution_amount":"1500.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":930498,
         "transaction_id":"C9900255",
         "entity_type":"PAC",
         "contributor_fec_id":"C00144766",
         "contributor_organization_name":"National Beer Wholesales Association PAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1101 King St",
         "contributor_street_2":"Ste 600",
         "contributor_city":"Alexandria",
         "contributor_state":"VA",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-06-10",
         "contribution_amount":"2500.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":930498,
         "transaction_id":"C9900263",
         "entity_type":"PAC",
         "contributor_fec_id":"C00012468",
         "contributor_organization_name":"The Coca-Cola Company",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1 COCA-COLA PLAZA NW",
         "contributor_street_2":null,
         "contributor_city":"ATLANTA",
         "contributor_state":"GA",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-06-10",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      },
      {
         "cycle":2014,
         "fec_committee_id":"C00415331",
         "fec_filing_id":930498,
         "transaction_id":"C9897888",
         "entity_type":"PAC",
         "contributor_fec_id":"C00364778",
         "contributor_organization_name":"BANK OF AMERICA CORPORATION FEDERAL PAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1455 Pennsylvania Ave NW",
         "contributor_street_2":"DC8-455-09-01",
         "contributor_city":"Washington",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2014-06-09",
         "contribution_amount":"1500.0",
         "fec_candidate_id":"H4NY11138",
         "office_state":"NY"
      }
   ],
   "callback":null
}
```

During the last 20 days before a primary or general election, candidate committees must file reports of any contributions of $1,000 or more within 48 hours of receipt. This endpoint retrieves the most recent late contributions to a specific candidate.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/candidates/{fec-id}/48hour`

### Query Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a committee. To find a candidate's official FEC ID, use a candidate search request or the [FEC web site](http://www.fec.gov).

## Get Recent Late Contributions to a Specific Committee

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/committees/C00575050/48hour.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "offset":null,
   "results":[
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024237,
         "transaction_id":"65C903E1837EE4CC8AF4",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"THE FARM CREDIT COUNCIL POLITICAL ACTION COMMITTEE",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"50 F STREET NW",
         "contributor_street_2":"SUITE 900",
         "contributor_city":"WASHINGTON",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-07",
         "contribution_amount":"2000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024237,
         "transaction_id":"6DE024C3050E240A79B1",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"NRA Political Victory Fund",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"11250 Waples Mill Rd",
         "contributor_street_2":null,
         "contributor_city":"Fairfax",
         "contributor_state":"VA",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-07",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024159,
         "transaction_id":"6739A98EA4D6B4888B6D",
         "entity_type":"IND",
         "contributor_fec_id":null,
         "contributor_organization_name":null,
         "contributor_prefix":null,
         "contributor_first_name":"Daniel",
         "contributor_middle_name":null,
         "contributor_last_name":"Gallagher",
         "contributor_suffix":null,
         "contributor_street_1":"1454 Olive Rd",
         "contributor_street_2":null,
         "contributor_city":"Homewood",
         "contributor_state":"IL",
         "contributor_zip":null,
         "contributor_employer":"Information Requested",
         "contributor_occupation":"Information Requested",
         "contribution_date":"2015-09-05",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024159,
         "transaction_id":"6C1DC0CA5724F4C7B86F",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"ALTRIA GROUP, INC. POLITICAL ACTION COMMITTEE (ALTRIAPAC)",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"101 CONSTITUTION AVE NW",
         "contributor_street_2":"SUITE 400W",
         "contributor_city":"WASHINGTON",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-05",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024159,
         "transaction_id":"6DFB4ABAF3C0747E39F3",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"FMC CORPORATION GOOD GOVERNMENT PROGRAM",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1050 K STREET, NW",
         "contributor_street_2":"SUITE 600",
         "contributor_city":"WASHINGTON",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-05",
         "contribution_amount":"2000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024159,
         "transaction_id":"6927FCB14CA3846CC947",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"Eye of the Tiger PAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"PO Box 2485",
         "contributor_street_2":null,
         "contributor_city":"Springfield",
         "contributor_state":"VA",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-05",
         "contribution_amount":"5000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024076,
         "transaction_id":"69E8D217D148741CFA22",
         "entity_type":"IND",
         "contributor_fec_id":null,
         "contributor_organization_name":null,
         "contributor_prefix":null,
         "contributor_first_name":"Peter",
         "contributor_middle_name":null,
         "contributor_last_name":"Fisher",
         "contributor_suffix":null,
         "contributor_street_1":"114 Glenridge Dr",
         "contributor_street_2":null,
         "contributor_city":"East Peoria",
         "contributor_state":"IL",
         "contributor_zip":null,
         "contributor_employer":"State of Illinois",
         "contributor_occupation":"Prisoner Review Board Member",
         "contribution_date":"2015-09-04",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024029,
         "transaction_id":"63D1AA766A9774C98B5D",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"COMMUNITY BANKERS ASSOCIATION OF ILLINOIS FEDPAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"901 COMMUNITY DRIVE",
         "contributor_street_2":null,
         "contributor_city":"Springfield",
         "contributor_state":"IL",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-02",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024029,
         "transaction_id":"6BB7B7691DCA7490D85A",
         "entity_type":"IND",
         "contributor_fec_id":null,
         "contributor_organization_name":null,
         "contributor_prefix":null,
         "contributor_first_name":"William",
         "contributor_middle_name":null,
         "contributor_last_name":"Pape",
         "contributor_suffix":null,
         "contributor_street_1":"11230 Oak Trail Drive",
         "contributor_street_2":null,
         "contributor_city":"Peoria",
         "contributor_state":"IL",
         "contributor_zip":null,
         "contributor_employer":"Information Requested",
         "contributor_occupation":"Information Requested",
         "contribution_date":"2015-09-02",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024029,
         "transaction_id":"683E8888AC022415A884",
         "entity_type":"IND",
         "contributor_fec_id":null,
         "contributor_organization_name":null,
         "contributor_prefix":null,
         "contributor_first_name":"Michael",
         "contributor_middle_name":null,
         "contributor_last_name":"Steelman",
         "contributor_suffix":null,
         "contributor_street_1":"1313 Washington",
         "contributor_street_2":null,
         "contributor_city":"Bushnell",
         "contributor_state":"IL",
         "contributor_zip":null,
         "contributor_employer":"Farmers and Merchants State Bank",
         "contributor_occupation":"Banker",
         "contribution_date":"2015-09-02",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024029,
         "transaction_id":"6F1B57C8368FE4C8184E",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"PETROLEUM MARKETERS ASSOCIATION OF AMERICAN\\SMALL BUSINESS COMMITTEE",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1901 NORTH FORT MYER DRIVE",
         "contributor_street_2":"SUITE 500",
         "contributor_city":"Arlington",
         "contributor_state":"VA",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-02",
         "contribution_amount":"1500.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024029,
         "transaction_id":"6E922530EF14E4D00B43",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"National Electrical Contractors Association PAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"3 Bethesda Metro Center Suite #110",
         "contributor_street_2":null,
         "contributor_city":"Bethesda",
         "contributor_state":"MD",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-02",
         "contribution_amount":"2500.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024029,
         "transaction_id":"6545F7F66ED494E22A41",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"Union Pacific Corporation Fund for Effective Government",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"700 13th St NW",
         "contributor_street_2":"Suite 350",
         "contributor_city":"Washington",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-02",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1023976,
         "transaction_id":"632DD3C846DB34785924",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"ARAB AMERICAN LEADERSHIP COUNCIL PAC",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1600 K STREET NW SUITE 601",
         "contributor_street_2":null,
         "contributor_city":"WASHINGTON",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-01",
         "contribution_amount":"2000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1023880,
         "transaction_id":"6AD4FE210F59A4DDB820",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"BUILD POLITICAL ACTION COMMITTEE OF THE NATIONAL ASSOCIATION OF HOME BUILDERS (BUILDPAC)",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1201 15TH STREET, NW",
         "contributor_street_2":null,
         "contributor_city":"WASHINGTON",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-08-31",
         "contribution_amount":"2500.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1023880,
         "transaction_id":"6574676AF2A3046F0B69",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"EYEPAC POLITICAL ACTION COMMITTEE FOR AMERICAN SOCIETY OF CATARACT AND REFRACTIVE SURGERY",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"4000 LEGATO ROAD, SUITE 700",
         "contributor_street_2":null,
         "contributor_city":"FAIRFAX",
         "contributor_state":"VA",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-08-31",
         "contribution_amount":"2500.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1023880,
         "transaction_id":"6EAB1B3227E6348B39FE",
         "entity_type":"IND",
         "contributor_fec_id":null,
         "contributor_organization_name":null,
         "contributor_prefix":null,
         "contributor_first_name":"Sandra",
         "contributor_middle_name":null,
         "contributor_last_name":"Yeh-Kane",
         "contributor_suffix":null,
         "contributor_street_1":"4920 Foxhall Lane",
         "contributor_street_2":null,
         "contributor_city":"Springfield",
         "contributor_state":"IL",
         "contributor_zip":null,
         "contributor_employer":"Information Requested",
         "contributor_occupation":"Information Requested",
         "contribution_date":"2015-08-31",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1023880,
         "transaction_id":"6B8344EE57E914824877",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"American Council of Engineering Companies (ACEC PAC)",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"1015 15th Street, NW",
         "contributor_street_2":"8th floor",
         "contributor_city":"Washington",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-08-31",
         "contribution_amount":"2500.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1023880,
         "transaction_id":"6AC3D11195E184DCD94A",
         "entity_type":"IND",
         "contributor_fec_id":null,
         "contributor_organization_name":null,
         "contributor_prefix":null,
         "contributor_first_name":"Dennis",
         "contributor_middle_name":"P",
         "contributor_last_name":"LaHood",
         "contributor_suffix":null,
         "contributor_street_1":"1001 Highview Rd",
         "contributor_street_2":null,
         "contributor_city":"East Peoria",
         "contributor_state":"IL",
         "contributor_zip":null,
         "contributor_employer":"None",
         "contributor_occupation":"Retired",
         "contribution_date":"2015-08-31",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1023880,
         "transaction_id":"63CA7C8D8BEAB4EA5B9B",
         "entity_type":"IND",
         "contributor_fec_id":null,
         "contributor_organization_name":null,
         "contributor_prefix":null,
         "contributor_first_name":"Timothy",
         "contributor_middle_name":null,
         "contributor_last_name":"Vanfleet",
         "contributor_suffix":null,
         "contributor_street_1":"3800 Vanderbilt Cir.",
         "contributor_street_2":null,
         "contributor_city":"Springfield",
         "contributor_state":"IL",
         "contributor_zip":null,
         "contributor_employer":"Self Employed",
         "contributor_occupation":"Physician",
         "contribution_date":"2015-08-31",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      }
   ],
   "callback":null
}
```

During the last 20 days before a primary or general election, candidate committees must file reports of any contributions of $1,000 or more within 48 hours of receipt. This endpoint retrieves the most recent late contributions to a specific committee.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/committees/{fec-id}/48hour`

### Query Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a committee. To find a candidate's official FEC ID, use a candidate search request or the [FEC web site](http://www.fec.gov).

## Get Recent Late Contributions by Date

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/contributions/48hour/2015/09/07.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "offset":null,
   "results":[
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024237,
         "transaction_id":"65C903E1837EE4CC8AF4",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"THE FARM CREDIT COUNCIL POLITICAL ACTION COMMITTEE",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"50 F STREET NW",
         "contributor_street_2":"SUITE 900",
         "contributor_city":"WASHINGTON",
         "contributor_state":"DC",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-07",
         "contribution_amount":"2000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00575050",
         "fec_filing_id":1024237,
         "transaction_id":"6DE024C3050E240A79B1",
         "entity_type":"PAC",
         "contributor_fec_id":null,
         "contributor_organization_name":"NRA Political Victory Fund",
         "contributor_prefix":null,
         "contributor_first_name":null,
         "contributor_middle_name":null,
         "contributor_last_name":null,
         "contributor_suffix":null,
         "contributor_street_1":"11250 Waples Mill Rd",
         "contributor_street_2":null,
         "contributor_city":"Fairfax",
         "contributor_state":"VA",
         "contributor_zip":null,
         "contributor_employer":null,
         "contributor_occupation":null,
         "contribution_date":"2015-09-07",
         "contribution_amount":"1000.0",
         "fec_candidate_id":"H6IL18088",
         "office_state":"IL"
      }
   ],
   "callback":null
}
```

During the last 20 days before a primary or general election, candidate committees must file reports of any contributions of $1,000 or more within 48 hours of receipt. This endpoint retrieves late contributions from a specific date.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/contributions/48hour/{year}/{month}/{day}`

### Query Parameters

Parameter | Description
--------- | -----------
year | The four-digit year from 2001-2016
month | The two-digit month from 01-12
day | The two-digit day from 01-31

# Lobbyist Bundlers

## Get Lobbyist Bundlers for a Specific Committee

```shell
curl "https://api.propublica.org/campaign-finance/v1/2016/committees/C00579458/lobbyist_bundlers.json"
  -H "X-API-Key: PROPUBLICA_API_KEY"
```

> The above command returns JSON structured like this:

```json
{
   "status":"OK",
   "copyright":"Copyright (c) 2016 ProPublica Inc. All Rights Reserved.",
   "base_uri":"https://api.propublica.org/campaign-finance/v1/2016/",
   "cycle":2016,
   "results":[
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1029579,
         "transaction_id":"SA.39106543",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"",
         "bundler_first_name":"MATTHEW",
         "bundler_middle_name":"THOMAS",
         "bundler_last_name":"ECHOLS",
         "bundler_suffix":"",
         "bundler_street_1":"945 NORTHCLIFFE DR NW",
         "bundler_street_2":"",
         "bundler_city":"ATLANTA",
         "bundler_state":"GA",
         "bundler_zip":"303181639",
         "bundler_employer":"THE COCA-COLA COMPANY",
         "bundler_occupation":"SENIOR VICE PRESIDENT",
         "bundled_amount":"39900.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1029579,
         "transaction_id":"SA-39104721",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"MR.",
         "bundler_first_name":"GAYLORD",
         "bundler_middle_name":"T.",
         "bundler_last_name":"HUGHEY",
         "bundler_suffix":"JR.",
         "bundler_street_1":"305 FERRELL PL",
         "bundler_street_2":"",
         "bundler_city":"TYLER",
         "bundler_state":"TX",
         "bundler_zip":"757027109",
         "bundler_employer":"GAYLORD HUGHEY LAW",
         "bundler_occupation":"ATTORNEY/LOBBYIST",
         "bundled_amount":"43100.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1029579,
         "transaction_id":"SA-39026407",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"MR.",
         "bundler_first_name":"DIRK",
         "bundler_middle_name":"",
         "bundler_last_name":"VAN DONGEN",
         "bundler_suffix":"",
         "bundler_street_1":"2721 31ST ST NW",
         "bundler_street_2":"STE 300 ",
         "bundler_city":"WASHINGTON",
         "bundler_state":"DC",
         "bundler_zip":"200083522",
         "bundler_employer":"NATIONAL ASSOCIATION OF WHOLESALERS",
         "bundler_occupation":"EXECUTIVE",
         "bundled_amount":"24200.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1015098,
         "transaction_id":"SA-39005836",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"MR.",
         "bundler_first_name":"AL",
         "bundler_middle_name":"R.",
         "bundler_last_name":"CARDENAS",
         "bundler_suffix":"",
         "bundler_street_1":"200 SOUTH BISCAYNE BLVD",
         "bundler_street_2":"STE. 4100 ",
         "bundler_city":"MIAMI",
         "bundler_state":"FL",
         "bundler_zip":"331312362",
         "bundler_employer":"SQUIRE PATTON BOGGS (US) LLP",
         "bundler_occupation":"SENIOR PARTNER",
         "bundled_amount":"18900.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1015098,
         "transaction_id":"SA-39011605",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"MR.",
         "bundler_first_name":"TRE'",
         "bundler_middle_name":"",
         "bundler_last_name":"EVERS",
         "bundler_suffix":"",
         "bundler_street_1":"605 EAST ROBINSON STREET",
         "bundler_street_2":"SUITE 310 ",
         "bundler_city":"ORLANDO",
         "bundler_state":"FL",
         "bundler_zip":"328012040",
         "bundler_employer":"CONSENSUS COMMUNICATIONS",
         "bundler_occupation":"LOBBYIST AND POLITICAL CONSULTANT",
         "bundled_amount":"18900.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1015098,
         "transaction_id":"SA-39014257",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"MR.",
         "bundler_first_name":"WILLIAM",
         "bundler_middle_name":"G.",
         "bundler_last_name":"HARRISON",
         "bundler_suffix":"JR.",
         "bundler_street_1":"213 BUNKERS COVE ROAD",
         "bundler_street_2":"",
         "bundler_city":"PANAMA CITY",
         "bundler_state":"FL",
         "bundler_zip":"324013909",
         "bundler_employer":"HARRISON RIVARD DUNCAN & BUZZETT",
         "bundler_occupation":"ATTORNEY",
         "bundled_amount":"29600.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1015098,
         "transaction_id":"SA-39014901",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"MR.",
         "bundler_first_name":"RICHARD",
         "bundler_middle_name":"F.",
         "bundler_last_name":"HOHLT",
         "bundler_suffix":"",
         "bundler_street_1":"7901 KENT ROAD",
         "bundler_street_2":"",
         "bundler_city":"ALEXANDRIA",
         "bundler_state":"VA",
         "bundler_zip":"223081328",
         "bundler_employer":"SELF-EMPLOYED",
         "bundler_occupation":"CONSULTANT",
         "bundled_amount":"27000.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1015098,
         "transaction_id":"SA-39042816",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"MR.",
         "bundler_first_name":"WILLIAM",
         "bundler_middle_name":"P.",
         "bundler_last_name":"KILLMER",
         "bundler_suffix":"",
         "bundler_street_1":"900 EMERALD DRIVE",
         "bundler_street_2":"",
         "bundler_city":"ALEXANDRIA",
         "bundler_state":"VA",
         "bundler_zip":"22308    ",
         "bundler_employer":"MORTGAGE BANKING ASSOCIATION",
         "bundler_occupation":"SENIOR VICE PRESIDENT",
         "bundled_amount":"36200.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1015098,
         "transaction_id":"SA-39041218",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"HON.",
         "bundler_first_name":"THOMAS",
         "bundler_middle_name":"G.",
         "bundler_last_name":"LOEFFLER",
         "bundler_suffix":"",
         "bundler_street_1":"5200 KELLER SPRINGS",
         "bundler_street_2":"SUITE 825 ",
         "bundler_city":"DALLAS",
         "bundler_state":"TX",
         "bundler_zip":"752482746",
         "bundler_employer":"AKIN GUMP",
         "bundler_occupation":"SR. PARTNER",
         "bundled_amount":"31500.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1015098,
         "transaction_id":"SA-39041478",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"MR.",
         "bundler_first_name":"IGNACIO",
         "bundler_middle_name":"E.",
         "bundler_last_name":"SANCHEZ",
         "bundler_suffix":"",
         "bundler_street_1":"500 8TH STREET NW",
         "bundler_street_2":"",
         "bundler_city":"WASHINGTON",
         "bundler_state":"DC",
         "bundler_zip":"200042131",
         "bundler_employer":"DLA PIPER",
         "bundler_occupation":"PARTNER",
         "bundled_amount":"32400.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1015098,
         "transaction_id":"SA-39026407",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"MR.",
         "bundler_first_name":"DIRK",
         "bundler_middle_name":"",
         "bundler_last_name":"VAN DONGEN",
         "bundler_suffix":"",
         "bundler_street_1":"2721 31ST STREET NORTHWEST",
         "bundler_street_2":"SUITE 300 ",
         "bundler_city":"WASHINGTON",
         "bundler_state":"DC",
         "bundler_zip":"200083522",
         "bundler_employer":"NATIONAL ASSOCIATION OF WHOLESALERS",
         "bundler_occupation":"EXECUTIVE",
         "bundled_amount":"33900.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1029579,
         "transaction_id":"SA-39040840",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"MS.",
         "bundler_first_name":"MARIA",
         "bundler_middle_name":"",
         "bundler_last_name":"CINO",
         "bundler_suffix":"",
         "bundler_street_1":"723 S UNION ST",
         "bundler_street_2":"",
         "bundler_city":"ALEXANDRIA",
         "bundler_state":"VA",
         "bundler_zip":"223143889",
         "bundler_employer":"HEWLETT-PACKARD",
         "bundler_occupation":"GOVERNMENT RELATIONS",
         "bundled_amount":"18025.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1029579,
         "transaction_id":"SA-39014901",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"MR.",
         "bundler_first_name":"RICHARD",
         "bundler_middle_name":"F.",
         "bundler_last_name":"HOHLT",
         "bundler_suffix":"",
         "bundler_street_1":"7901 KENT RD",
         "bundler_street_2":"",
         "bundler_city":"ALEXANDRIA",
         "bundler_state":"VA",
         "bundler_zip":"223081328",
         "bundler_employer":"SELF-EMPLOYED",
         "bundler_occupation":"CONSULTANT",
         "bundled_amount":"27400.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      },
      {
         "cycle":2016,
         "fec_committee_id":"C00579458",
         "fec_filing_id":1029579,
         "transaction_id":"SA-39188060",
         "entity_type":"IND",
         "bundler_fec_id":null,
         "bundler_organization_name":"",
         "bundler_prefix":"MR.",
         "bundler_first_name":"WILLIAM",
         "bundler_middle_name":"P.",
         "bundler_last_name":"KILLMER",
         "bundler_suffix":"",
         "bundler_street_1":"1919 M ST NW",
         "bundler_street_2":"5TH FLOOR",
         "bundler_city":"WASHINGTON",
         "bundler_state":"DC",
         "bundler_zip":"200363521",
         "bundler_employer":"MORTGAGE BANKERS ASSOCIATION",
         "bundler_occupation":"SENIOR VICE PRESIDENT - POLITICAL AFF.",
         "bundled_amount":"26900.0",
         "start_date":"2016-01-22",
         "end_date":"2016-01-22"
      }
   ]
}
```

Committees must report registered lobbyists who act as "bundlers", collecting donations for the committee from multiple contributors. This endpoint retrieves the most recent lobbyist bundlers reported by a specific committee.

### HTTP Request

`GET https://api.propublica.org/campaign-finance/v1/{cycle}/committees/{fec-id}/lobbyist_bundlers`

### Query Parameters

Parameter | Description
--------- | -----------
fec-id | The FEC-assigned 9-character ID of a committee. To find a candidate's official FEC ID, use a candidate search request or the [FEC web site](http://www.fec.gov).
