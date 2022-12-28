# VoterVoice

**Welcome to the VoterVoice REST API!**

In order to use the API, you need to have purchased an advocacy package from VoterVoice and have access to the authentication key for the API. If you do not have your authentication key, please get in touch with your Account Manager. *Do not share this authentication key* as it identifies API usage by your organization.

More documentation and a request sandbox is available in our <a href="https://votervoice.docs.apiary.io/">VoterVoice API Docs</a>.

## API URL
```
https://www.votervoice.net/api/
```

# Authentication

## Headers

All requests must include the following headers:

```
    Content-Type  : application/json; charset=utf-8
    Authorization : YOUR_API_KEY
```

All calls to the API are required to have a Request Header with the name "Authorization" filled in with the authentication key that was provided to you.

`Authorization : YOUR_API_KEY`

**Always** send your authorization key over HTTPS. If you send any requests over HTTP, *they will be rejected*.

The authorization key will be denoted by `YOUR_API_KEY` in the examples. Please replace it with your association's API key.

## Association Name

Many calls will require you to provide your association/organization identification name in the query string.
This is an alphabetic identifier for your association. This should be provided by your account manager at VoterVoice.

The association name will be denoted by `YOUR_ASSOCIATION_IDNAME` in the examples. Please replace it with your association's id name.

Example: `?association=YOUR_ASSOCIATION_IDNAME`

## Authentication Redirects

In the event that an endpoint responds back with a 401 Unauthorized, but your authorization key is correct, 
the request may have been redirected, and the Authentication header stripped by your client. You can use the
Response URI received from the request to redirect to the proper endpoint. Below is an example in .NET on how 
this might be handled.

### An Example in C#

```
var apiKey = "YOUR_API_KEY";
var idName = "YOUR_ASSOCIATION_IDNAME";
var contactId = "contactId";
var baseUrl = "https://www.votervoice.net";
var contentType = "application/json";

var credentials = new NetworkCredential(apiKey, "");
var credentialCache = new CredentialCache();
credentialCache.Add(new Uri(url), "Basic", credentials);

var httpClientHandler = new HttpClientHandler();
httpClientHandler.Credentials = credentialCache;

var client = new HttpClient(httpClientHandler);
client.BaseAddress = new Uri(baseUrl);
client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue(contentType));

var resourceUrl = $"api/contacts?association={idName}&contactId={contactId}";

// contact being the representation of your Contact
var content = new StringContent(contact, System.Text.Encoding.UTF8, contentType);
var response = client.PutAsJsonAsync<object>(resourceUrl, content).Result;
```

### An Example in VB

```
Dim apiKey = "YOUR_API_KEY"
Dim idName = "YOUR_ASSOCIATION_IDNAME"
Dim contactId = "contactId"
Dim baseUrl = "https://www.votervoice.net"
Dim contentType = "application/json"

Dim credentials As New NetworkCredential(apiKey, "")
Dim credentialCache As New CredentialCache()
credentialCache.Add(New Uri(baseUrl), "Basic", credentials)

Dim httpClientHandler As New HttpClientHandler()
httpClientHandler.Credentials = credentialCache

Dim client As New HttpClient(httpClientHandler)
client.BaseAddress = New Uri(baseUrl)
client.DefaultRequestHeaders.Accept.Add(New Headers.MediaTypeWithQualityHeaderValue("application/json"))

Dim resourceUrl = $"api/contacts?association={idName}&contactId={contactId}"

' contact being the representation of your Contact
Dim content As New StringContent(contact, Text.Encoding.UTF8, contentType)
Dim response = client.PutAsync($"api/contacts?association={idName}&contactId={contactId}", content).Result
```
# Error States
The common [HTTP Response Status Codes](https://github.com/for-GET/know-your-http-well/blob/master/status-codes.md) are used.

# Acknowledgement Requirements
- You must acknowledge VoterVoice on any web page or application page in which page content makes use of VoterVoice API calls. Acknowledgement takes the form of the following "Powered by VoterVoice" logo: <img height="65" src="https://d3dkdvqff0zqx.cloudfront.net/groups/system/images/poweredby_votervoice.png">
- You must insert the VoterVoice Google Analytics markup on every page that requires the use of the "Powered By VoterVoice" logo. Please use the following markup right before the </body> tag to call Google Analytics from your pages.

```javascript
<script type="text/javascript">
    var _gaq = _gaq || [];
    _gaq.push(
              ['_setAccount', 'UA-9858999-1'],
              ['_trackPageview']
     );
 
    (function () {
        var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
        ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
        var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
    })();
</script>
```

# Key Terminology

## General

- Administrator: The person using the VoterVoice administrator portal to create actions, manage contacts, view reports, etc.
- Association: The organization or group using the VoterVoice system. (e.g. XYZ Association of America)

## Actions

- Advocacy Campaign: Consists of an alert explaining an issue, targets for users to send messages to, and a sample message to send.
- Target: The recipient of advocacy messages within an advocacy campaign. Targets may consist of officials, regulatory agencies, and more.
- Message: The advocacy message a user submits to targets within an advocacy campaign.
- Survey: Consists of an alert explaining an issue and survey questions for users to submit.
- Petition: Consists of an alert explaning an issue and petition text for users to add their signature to.
- Event: Consists of an alert explaning an issue, accommodations, travel details, and resources, allowing a user to register for the event.
- Scorecard: Consists of bill actions defined by administrators, the elected officials voting on those actions, and their score relative to how the association preferred the officials to vote.
- Blog: Consists of an informational blog post.
- Meeting: Consists of an office, attendees, details, and a meeting report.
- Public vs Private: Campaigns, Surveys, Petitions, Events, and Blog Posts can be publicly visible on the client's action center, or private so they can only be accessed via direct link.


## Legislative

- Address: A physical address, normally used in matching an individual to their elected officials. (e.g. 1600 Pennsylvania Ave NW, Washington, DC 20500)
- District: The legislative districts an individual matches to based on their address.
- Government: A collection of officials representing specific constituencies.

## People

- User: An individual registering by submitting their information on an action.
- Contact: Any individual in the association's contact database.
- Official: A publicly elected official, holding office within a government entity.
- UserToken: The API identifies a user by a user token. It is represented by the term USER_TOKEN in the documentation.

## Uploaded vs Registered Contacts

An administrator can bulk upload or individually create contacts in their VoterVoice system. We colloquially refer to these contacts as "uploaded" contacts. Those contacts have not yet taken action or validated their own information.

Once an uploaded contact interacts with the VoterVoice system themselves (by taking action on an advocacy campaign, registering for an event, etc.) we refer to them as "registered." Also known as "users."

## Date Range Parameters

For resources that accommodate a date querystring parameter for filtering, the supported semantics are as follows:

Greater than or equal to: `date=>yyyy-MM-ddTHH:mm:ssZ`
```
https://www.votervoice.net/api/resources?date=>2017-01-01T13:10:00Z
```

Less than or equal to: `date=<yyyy-MM-ddTHH:mm:ssZ`  
```
https://www.votervoice.net/api/resources?date=<2017-01-01T13:10:00Z
```

Equal to: `date=yyyy-MM-ddTHH:mm:ssZ`  
```
https://www.votervoice.net/api/resources?date=2017-01-01T13:10:00Z
```

The greater than and less than filters may be combined within the same request.
```
https://www.votervoice.net/api/resources?date=>2017-01-01T00:00:00Z&date=<2017-01-01T23:59:59Z
```