#### Generating Oauth 1.0 Signature ! 
[comment]:![_config.yml]({{ site.baseurl }}/images/config.png)

https://stackabuse.com/authentication-and-authorization-with-jwts-in-express-js/

##### Difference between Oauth 1.0 and 2.0 
OAuth 1.0 only handled web workflows, but OAuth 2.0 considers non-web clients as well. Better separation of duties. Handling resource requests and handling user authorization can be decoupled in OAuth 2.0. Basic signature workflow.

##### Introduce middleware
We introduced express middleware in order to perform this authentication
prior to calling the client application. you can choose to use serverless lambda express function for this as well. 
Use serverless typescript template for the same. 

##### How Oath Signatures are Generated 
Use oauth1.0 library mentioned below. 

##### Full code including constructing base string
//TODO: Need to create working code and share but this should give folks idea for now. 
```
    const consumer: OauthConsumerDomain = {
      key: process.env.APP_CONSUMER_KEY!,
      secret: process.env.APP_CONSUMER_SECRET!,
    };

    // TODO: Investigate issue with https in local environment
    const requrl = `${process.env.HTTPS_ENABLED! === '0' ? 'http' : 'https'}://${req.headers.host}${req.url}`;

    try {
      const expectedSignature = this.oauthSigner.calculateSignature(consumer, RequestUrl, Request);
      this.ltiBasicLaunchBll.validateBasicLaunch(Request, consumer, expectedSignature);

      return next();
    } catch (err) {
      return next(Boom.unauthorized('Unauthorized'));
    }
  }
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

A better way to generate signature and base string is by using library oauth1.0 
//TODO: Create proper code for this and check this in personal repo for others reference 
```
import OAuth = require('oauth-1.0a');

//create oath object 
  private createOAuth(consumer: OauthConsumerDomain, signatureMethod: string): OAuth {
    return new OAuth({
      consumer,
      signature_method: signatureMethod,
      hash_function: (baseString: string, key: string): string => {
        return crypto
          .createHmac('sha1', key)
          .update(baseString)
          .digest('base64');
          
//now form signature

  calculateSignature(consumer: OauthConsumerDomain, endpoint: string, requestParams: OauthRequestDomain): string {
    //do some validations

    return oauth.getSignature(
      {
        url: endpoint,
        method: 'POST',
        data: OauthSigner.extractSignableParams(requestParams),
      },
      undefined,
      {
        oauth_consumer_key: requestParams.oauth_consumer_key,
        oauth_nonce: requestParams.oauth_nonce,
        oauth_signature_method: requestParams.oauth_signature_method,
        oauth_timestamp: requestTimestamp,
        oauth_version: requestParams.oauth_version,
        oauth_token: '',
      },
    );
  }

```

And from this library calling 

##### How to match base string 
I tried a complex path but later realized basestring should not be constructed by yourself. It should be left over to oauth1.0 library to do it correctly.  

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
we need not use escape function since that is also part of forming base string. 


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
Above articles do not refer to the code i mentioned in latest version. The code snippet provided is the final implementation. 


#### After discussion with Perry
Made an entry in package.json for "oauth-1.0a": "^2.2.5",
yarn install 



