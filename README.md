# cloudflare-cors-anywhere
Cloudflare CORS proxy in a worker.

CLOUDFLARE-CORS-ANYWHERE

Source:
https://github.com/Zibri/cloudflare-cors-anywhere

## Deployment

This project is written in [Cloudflare Workers](https://workers.cloudflare.com/), and can be easily deployed with [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/install-and-update/).

1. Have a Cloudflare account
2. Change your `subdomain-name` to your desired subdomain name
   1. Go to Workers & Pages -> Overview
   2. Click **Change** on the Subdomain element to the right of the screen  
3. Clone this Git repository locally
4. Change `name` in `wrangler.toml` to your desired worker name.
5. `npm install wrangler --save-dev`
6. `npx wrangler deploy`
7. Now you can use your proxy as `name.subdomain-name/?uri`

## Usage Example

```javascript
fetch('https://test.cors.workers.dev/?https://httpbin.org/post', {
  method: 'post',
  headers: {
    'x-foo': 'bar',
    'x-bar': 'foo',
    'x-cors-headers': JSON.stringify({
      // allows to send forbidden headers
      // https://developer.mozilla.org/en-US/docs/Glossary/Forbidden_header_name
      'cookies': 'x=123'
    }) 
  }
}).then(res => {
  // allows to read all headers (even forbidden headers like set-cookies)
  const headers = JSON.parse(res.headers.get('cors-received-headers'))
  console.log(headers)
  return res.json()
}).then(console.log)
```

Note:

All received headers are also returned in "cors-received-headers" header.

All Thanks go to Zibri and the others working on this project