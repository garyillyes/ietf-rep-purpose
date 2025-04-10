Hung up on something you mentioned in passing, namely if we're
updating X-Robots-Tag then might as well try to move away from it.
We still need to support some of its old values along with the 
new ones, which makes things pretty ugly, but anyway, we MUST 
supoort in some way the GDPR controls but having two places for
specifying stuff for search is going to be a nightmare (for the
ecosystem). It seems like we should scrifice some of the clean 
design for ensuring that we don't confuse the ecosystem too much.

i.e. I'd give up on `robots-tag` header if we can figure out how
to shove (some of) its things into a new content dash header to
ensure there we're not having two places to specify aiprefs, at
least at launch time.

HTTP response for `user-agent` Bingbot, with `vary` and the `search`
vocab:

~~~
404 Not Found
Date: Mon, 17 Feb 2025 03:32:05 GMT
Vary: user-agent
Content-Usage: tdm=y,ai=n,search=y
Content-Type: text/html

<HTML page contents omitted from this example>
~~~

Same HTTP response for `user-agent` Bingbot, but this time the 
`search` vocab is a list with GDPR flags (this is violating 
rfc9651 tough, I think):

~~~
404 Not Found
Date: Mon, 17 Feb 2025 03:32:05 GMT
Vary: user-agent
Content-Usage: tdm=y,ai=n,search=(y;max-snippet=42;max-image-preview=large)
Content-Type: text/html

<HTML page contents omitted from this example>
~~~

HTTP response for `user-agent` Googlebot under the same URL:

~~~
404 Not Found
Date: Mon, 17 Feb 2025 03:32:05 GMT
Vary: user-agent
Content-Usage: tdm=y,ai=n,search=n
Content-Type: text/html

<HTML page contents omitted from this example>
~~~

With the `search` vocab we may be able to get rid of `noindex`, and 
technically `nosnippet` is `max-snippet=0`. There's a couple that are
weird, like `nofollow` and `notranslate` and Bing's `nocache`; no
clue yet how would I deal with that.

robots.txt example stays the same, I think that's good to go.

~~~
User-Agent: *
Usage: tdm=n,search=y
Allow: /article/
~~~
