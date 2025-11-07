## Authorizing in postman

### Embedding token to all requests in a collection

#### In a collection, go to variables and create a variable without any value. you can name it *access_token*.

![Alt text](img/img.png?raw=True)

<br />

#### On Scripts > Pre-Request, create a script to get the token. you can use postman's pm object <br /> like in example. Again, only work if it is saved

```javascript
const clientId = "<--clientId-->"
const clientSecret = "<--clientSecret-->"
const scope = "<--scope-->"
const tokenUrl = "https://<--tokenUrl-->/oauth2/token"

const credentials = `${clientId}:${clientSecret}`;
const encodedCredentials = Buffer.from(credentials).toString('base64');

const getTokenRequest = {
  method: 'POST',
  url: tokenUrl,
  header: {
      'Authorization': `Basic ${encodedCredentials}`,
      'Content-Type': 'application/x-www-form-urlencoded'
  },
  body: {
      mode: 'raw',
      raw: `grant_type=client_credentials&scope=${scope}`
  }
};

pm.sendRequest(getTokenRequest, (err, response) => {
  const jsonResponse = response.json();
  const newAccessToken = jsonResponse.access_token;

  pm.variables.set('access_token', newAccessToken);
});
```

- Replace the variables with actual clientID, clientSecret, scope and tokenUrl. 
- This script assigns the token to *access_token* variable created before. 
- Some token request structures may be slightly different. In some cases the client credentials must be <br /> sent in the body

<br />

#### Set the Authorization on the collection as bearer token and put the *access_token* variable <br /> the syntax is double curly braces as follows

![Alt text](img/img_1.png?raw=True)

<br />

#### Finally, on the requests inside the collection, use Authorization > Inherit auth from parent

![Alt text](img/img_2.png?raw=True)

#### Now, the requests should be authorized automatically

##### In case of errors, some steps may help debug:

- Verify Credentials
- Check Token URL
- Check any error logged

- Confirm res.json().access_token exists
