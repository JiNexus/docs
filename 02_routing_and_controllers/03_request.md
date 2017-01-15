# Request

--------------------------------------------------------

* [Accessing data](#accessing_data)
* [Reading cookies](#reading_cookies)
* [Request information](#request_information)

--------------------------------------------------------

The request class provides an object oriented interface to global variables such as ```$_GET```, ```$_POST```, ```$_FILES```, ```$_COOKIE``` and ```$_SERVER``` as well as easy access to request data and other useful request information.

An instance of the request class is always available in all controller classes. It is also easily made available in [route closures](:base_url:/docs/:version:/routing-and-controllers:routing#basics).

--------------------------------------------------------

<a id="accessing_data"></a>

### Accessing data

The ```get``` method allows you to access query string data.

	// Get all query string data (returns an empty array if there is no data)

	$all = $this->request->get();

	// Get data from a specific key (will return NULL if the key doesn't exist)

	$foo = $this->request->get('foo');

	// Get data from a specific key and return FASLE if the key doesn't exist

	$foo = $this->request->get('foo', false);

The ```post``` method allows you to access ```POST``` data.

	// Get all POST data (returns an empty array if there is no data)

	$all = $this->request->post();

	// Get data from a specific key (will return NULL if the key doesn't exist)

	$foo = $this->request->post('foo');

	// Get data from a specific key and return FASLE if the key doesn't exist

	$foo = $this->request->post('foo', false);

The ```put``` method allows you to access ```PUT``` data.

> The method expects the request body to be one of the following types; ```application/x-www-form-urlencoded```, ```text/json``` or ```application/json```. If you want to access the raw request body then you'll have to use the ```body``` method instead.

	// Get all PUT data (returns an empty array if there is no data)

	$all = $this->request->put();

	// Get data from a specific key (will return NULL if the key doesn't exist)

	$foo = $this->request->put('foo');

	// Get data from a specific key and return FASLE if the key doesn't exist

	$foo = $this->request->put('foo', false);

The ```patch``` method allows you to access ```PATCH``` data.

> The method expects the request body to be one of the following types; ```application/x-www-form-urlencoded```, ```text/json``` or ```application/json```. If you want to access the raw request body then you'll have to use the ```body``` method instead.

	// Get all PATCH data (returns an empty array if there is no data)

	$all = $this->request->patch();

	// Get data from a specific key (will return NULL if the key doesn't exist)

	$foo = $this->request->patch('foo');

	// Get data from a specific key and return FASLE if the key doesn't exist

	$foo = $this->request->patch('foo', false);

The ```delete``` method allows you to access ```DELETE``` data.

> The method expects the request body to be one of the following types; ```application/x-www-form-urlencoded```, ```text/json``` or ```application/json```. If you want to access the raw request body then you'll have to use the ```body``` method instead.

	// Get all DELETE data (returns an empty array if there is no data)

	$all = $this->request->delete();

	// Get data from a specific key (will return NULL if the key doesn't exist)

	$foo = $this->request->delete('foo');

	// Get data from a specific key and return FASLE if the key doesn't exist

	$foo = $this->request->delete('foo', false);

The ```body``` method allows you to access the raw request body.

	$body = $this->request->body();

The ```bodyAsStream``` method returns the raw request body as a stream. This can be useful if you want to minimize the memory footprint of your application while handling large file uploads via PUT requests.

	$body = $this->request->bodyAsStream();

	$storageStream = fopen($this->app->getPath() . '/storage/uploads/' . $fileName, 'w');

	$filesize = stream_copy_to_stream($body, $storageStream);

The ```data``` method allows you to access data from the current request method.

	// Get all request data (returns an empty array if there is no data)

	$all = $this->request->data();

	// Get data from a specific key (will return NULL if the key doesn't exist)

	$foo = $this->request->data('foo');

	// Get data from a specific key and return FASLE if the key doesn't exist

	$foo = $this->request->data('foo', false);

The ```has``` method lets you check if a data key exist.

	$exists = $this->request->has('foo');

The ```whitelisted``` methods returns all request data where keys not in the whitelist have been removed.

	// Returns input data where everything except 'foo' and 'bar' keys have been removed

	$whitelisted = $this->request->whitelisted(['foo', 'bar']);

	// You can also specify default values that will be used if the keys don't exist

	$whitelisted = $this->request->whitelisted(['foo', 'bar'], ['foo' => 'baz', 'bar' => 'bax']);

The ```blacklisted``` methods returns all request data where keys in the blacklist have been removed.

	// Returns input data where the 'foo' and 'bar' keys have been removed

	$blacklisted = $this->request->blacklisted(['foo', 'bar']);

	// You can also specify default values that will be used if the keys don't exist

	$blacklisted = $this->request->blacklisted(['foo', 'bar'], ['baz' => 'foo', 'bax' => 'bar']);

The ```file``` method allows you to access files that have been upload via a POST request.

	// Get all uploaded files (returns an empty array if there is no data)

	$files = $this->request->file();

	// Get a specific file or an array of files if it's a multi-upload.

	$file = $this->request->file('myfile');

	// If you want to get a specific file from a multi-upload then you'll have to add the array key as well

	$files = $this->request->file('myfile.0');

Uploaded files are represented as a ```UploadedFile``` object. The class extends the ```SplFileInfo``` class with the following methods.

| Method name     | Description                                                                |
|-----------------|----------------------------------------------------------------------------|
| getName         | Returns the name of the file that was uploaded.                            |
| getReportedSize | Returns the size reported by the client.                                   |
| getReportedType | Returns the mime type reported by the client.                              |
| hasError        | Returns TRUE if there is an error with the uploaded file and FALSE if not. |
| getErrorCode    | Returns the error code associated with the error.                          |
| getErrorMessage | Returns a human friendly error message.                                    |
| isUploaded      | Returns TRUE if the file is an actual upload and FALSE if not.             |
| moveTo          | Moves the file to the specified storage location.                          |

> The values reported by the client should not be trusted. Use the ```getSize``` method to retrieve the actual filesize.

The ```server``` method lets you access server data.

	// Get all server data (returns an empty array if there is no data)

	$all = $this->request->server();

	// Get data from a specific key (will return NULL if the key doesn't exist)

	$foo = $this->request->server('SERVER_PORT');

	// Get data from a specific key and return '80' if the key doesn't exist

	$foo = $this->request->server('SERVER_PORT', 80);

--------------------------------------------------------

<a id="reading_cookies"></a>

### Reading cookies

The ```cookie``` method will return the value of the selected cookie.

	// Returns the value of the chosen cookie (returns NULL if the cookie doesn't exist)

	$value = $this->request->cookie('chocolate');

	// Return the value of the chosen cookie and return FASLE if the cookie doesn't exist

	$value = $this->request->cookie('chocolate', false);

The ```signedCookie``` method will return the value of the selected signed cookie.

	// Returns the value of the chosen cookie (returns NULL if the cookie doesn't exist)

	$value = $this->request->signedCookie('chocolate');

	// Return the value of the chosen cookie and return FASLE if the cookie doesn't exist

	$value = $this->request->signedCookie('chocolate', false);

> The benefit of using signed cookies over regular cookies is that their values can not be tampered with on the client site.

Setting regular and signed cookies is done using the [response class](:base_url:/docs/:version:/routing-and-controllers:response).

--------------------------------------------------------

<a id="request_information"></a>

### Request information

The ```header``` method returns the value of the chosen request header.

	// Returns the value of the chosen header (returns NULL if the header doesn't exist)

	$value = $this->request->header('content-type');

	// Returns the value of the chosen header and returns 'text/plain' if the header doesn't exist

	$value = $this->request->header('content-type', 'text/plain');

The ```referer``` method returns the address that referred the client to the current resource. NULL is returned if there is no referrer.

	// Returns the referrer if there is one and NULL if not

	$referer = $this->request->referer();

	// Returns the referrer if there is one and the URL to the 'home' route if there isn't one

	$referer = $this->request->referer($this->urlBuilder->toRoute('home'));

The ```acceptableContentTypes``` returns an array of acceptable content types in descending order of preference.

	$acceptableContentTypes = $this->request->acceptableContentTypes();

The ```acceptableLanguages``` returns an array of acceptable languages in descending order of preference.

	$acceptableLanguages = $this->request->acceptableLanguages();

The ```acceptableCharsets``` returns an array of acceptable character sets in descending order of preference.

	$acceptableCharsets = $this->request->acceptableCharsets();

The ```acceptableEncodings``` returns an array of acceptable encodings sets in descending order of preference.

	$acceptableEncodings = $this->request->acceptableEncodings();

The ```ip``` method returns IP of the client that made the request.

	$ip = $this->request->ip();

The ```isAjax``` method returns TRUE if the request was made using AJAX and FALSE if not.

	$isAjax = $this->request->isAjax();

The ```isSecure``` method returns TRUE if the request was made using HTTPS and FALSE if not.

	$isSecure = $this->request->isSecure();

The ```isSafe``` method returns TRUE if the request method used is considered safe and FALSE if not. The methods that are considered safe are ```GET``` and ```HEAD```.

	$isSafe = $this->request->isSafe();

The ```baseURL``` method returns the base URL of the request.

	// A request to 'http://example.org/foo/bar' will return 'http://example.org'

	$baseURL = $this->request->baseURL();

The ```path``` method will return the request path.

	// A request to 'http://example.org/foo/bar' will return '/foo/bar'

	$path = $this->request->path();

The ```language``` method returns the request language (see the ```app/config/application.php``` config file for more information).

	$language = $this->request->language();

The ```languagePrefix``` method returns the language prefix (see the ```app/config/application.php``` config file for more information).

	$languagePrefix = $this->request->languagePrefix();

The ```method``` method returns the request method used to access the resource.

	$method = $this->request->method();

The ```realMethod``` method returns the real request method used to access the resource.

	$method = $this->request->realMethod();

The ```isFaked``` method returns TRUE if the request method method has been faked and FALSE if not.

	$isFaked = $this->request->isFaked();

The ```username``` method returns a basic HTTP authentication username of NULL if it isn't set.

	$username = $this->request->username();

The ```password``` method returns a basic HTTP authentication password of NULL if it isn't set.

	$password = $this->request->password();

The ```getRoute``` method returns the route that matched the request.

	$route = $this->request->getRoute();
