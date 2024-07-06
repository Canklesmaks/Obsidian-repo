## encoding

It is essential to ensure that our request data is URL-encoded and our request headers are correctly set. Otherwise, we may get a server error in the response. This is why encoding and decoding data becomes essential as we modify and repeat web requests. Some of the key characters we need to encode are:
- `Spaces`: May indicate the end of request data if not encoded
- `&`: Otherwise interpreted as a parameter delimiter
- `#`: Otherwise interpreted as a fragment identifier

To URL-encode text in Burp Repeater, we can select that text and right-click on it, then select (`Convert Selection>URL>URL encode key characters`), or by selecting the text and clicking [`CTRL+U`].

* Burp also supports URL-encoding as we type if we right-click and enable that option, which will encode all of our text as we type it.
	* There are other types of URL-encoding, like `Full URL-Encoding` or `Unicode URL` encoding, which may also be helpful for requests with many special characters.

## decoding

URL-encoding is key to HTTP requests, it is not the only type of encoding we will encounter.

It is very common for web applications to encode their data, so we should be able to quickly decode that data to examine the original text.

 back-end servers may expect data to be encoded in a particular format or with a specific encoder, so we need to be able to quickly encode our data before we send it.


The following are some of the other types of encoders supported by both tools:

- HTML
- Unicode
- Base64
- ASCII hex

To access the full encoder in Burp, we can go to the `Decoder` tab.

![[Pasted image 20240506140725.png]]

We dfound things can be double encoded as base64, or as any type. we decoded as base64 3 times before getting a url encoded string.