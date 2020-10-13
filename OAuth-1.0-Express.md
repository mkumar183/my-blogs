#### Generating Oauth 1.0 Signature ! 


[comment]:![_config.yml]({{ site.baseurl }}/images/config.png)

https://stackabuse.com/authentication-and-authorization-with-jwts-in-express-js/

##### Difference between Oauth 1.0 and 2.0 
OAuth 1.0 only handled web workflows, but OAuth 2.0 considers non-web clients as well. Better separation of duties. Handling resource requests and handling user authorization can be decoupled in OAuth 2.0. Basic signature workflow.

##### Introduce middleware
We introduced express middleware in order to perform this authentication
prior to calling the client application. In our case client application
is rendered at server side so we did not have to pass any tokens
to this application. 

##### How Oath Signatures are Generated 
Oath signature is generated based on below method: <br>

```
   const oauth_signature = crypto  
     .createHmac('sha1', signing_key) 
     .update(signature_base_string) 
     .digest() 
     .toString('base64'); 
```

##### Full code including constructing base string
```
    const encodedKey = encodeURIComponent(k);
    const encodedValue = escape(ordered[k]); // in our case had to skip certain elements 

    // in our case had to re-encode certain parameters
    if (encodedParameters === '') {
      encodedParameters += encodeURIComponent(`${encodedKey}=${encodedValue}`);
    } else {
      encodedParameters += encodeURIComponent(`&${encodedKey}=${encodedValue}`);
    }

  const method = 'POST';
  const base_url = 'http://localhost:3000/';
  const encodedUrl = encodeURIComponent(base_url);
  const signature_base_string = `${method}&${encodedUrl}&${encodedParameters}`;

  const secret_key = `yoursecretkey`; // this needs to be moved to config later
  const signing_key = `${secret_key}&`; //as token is missing in our case.
  const crypto = require('crypto');

  const oauth_signature = crypto
    .createHmac('sha1', signing_key)
    .update(signature_base_string)
    .digest()
    .toString('base64');
```

Its important here to know how the signature_base_string is formed. 
In our case, this was formed by http method (POST or GET), url that 
is to be called (redirect url) and then all the parameters that 
form post is doing. How signature is constructed should be published by the receiver.
This was the main issue in our implementation. There were several
parameters that were re-encoded and that ended up inserting additional 
characters. 

Below url helped me in doing some quick experiments and trying to match 
string used by calling application. 
[Encoding](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent#:~:text=The%20encodeURIComponent()%20function%20encodes,two%20%22surrogate%22%20characters)  

##### How to match base string 
In order match base string, i used vimdiff. I put two windows side by side
and ensure their width was same by matching first line (fortunately 
first line was matching). Then we matched line by line (since its all wrapped)
so this was a laborious exercise. But by looking at each parameters we could 
figure out where the problems were. Using above url we encoded twice and checked
if it was a double encoding problem. 

##### Debugging signature mismatch
Put debug points at: 
- s_app_lti_app_launch in s_app_lti.inc 
- s_app_lti_app_launch_if_content_selection (line 470 in same file)
- OAuthRequestSignerLTI is called from generateLtiLaunchParams in ExternaltoolBill.php
- file OauthRequest.php line 211 signature. Capture base_string value in a notepad. 
- match this base_string with the base_string you are forming in your program using vimdiff editor side by side
- capture base_string from both the places open two vi editors such that line 
length is same then match first and last character of each line.
- Debug point at /Users/kumarman/sgy-projects/envy/systems/schoology/lib/sgy-shared/src/sgy-core/schoology/sites/all/misc/oauth/library/OAuthRequest.php line 186 and set the base string to validate.



##### escape function 
This function replaces colon (:) by %3A. 


##### Two legged vs Three legged Approach
First, the legs refer to the roles involved. A typical OAuth flow involves three parties: the end-user (or resource owner), the client (the third-party application), and the server (or authorization server). So a 3-legged flow involves all three.

The term 2-legged is used to describe an OAuth-authenticated request without the end-user involved. Basically, it is a simple client-server authenticated request in which the client credentials (identifier and secret) are used to calculate a request signature instead of sending the secret in the clear.

Implementation wise, 2-legged request are exactly the same but don't include an access token or access token secret. These two values are basically empty strings.
[reference link](https://stackoverflow.com/questions/5593348/difference-between-oauth-2-0-two-legged-and-three-legged-implementation#:~:text=A%20typical%20OAuth%20flow%20involves,without%20the%20end%2Duser%20involved.)



##### References <br>
Best Article: 
[Create Oath1.0 Signature](https://medium.com/@pandeysoni/how-to-create-oauth-1-0a-signature-in-node-js-7d477dead170) <br>
[Oath wp documentation](https://oauth1.wp-api.org/docs/advanced/Web.html) <br>
[Decode url](https://meyerweb.com/eric/tools/dencoder/)
[Encoding Experiment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent#:~:text=The%20encodeURIComponent()%20function%20encodes,two%20%22surrogate%22%20characters)  


#### After discussion with Perry
Made an entry in package.json for "oauth-1.0a": "^2.2.5",
yarn install 



