# Reading API Documentation

## Introduction 
We've now covered an example API request, but how on Earth would you know how to do that on your own? The answer is through documentation! All APIs will have associated documentation, and while there are substantial similarities, all will be different to one degree or another. The best way to get more comfortable is to practice! So with that, let's take a look at the yelp documentation associated with our previous example.  

## Objectives

You will be able to:

* Interpret API documentation into Python code
* Make requests using OAuth

Start by navigating to https://www.yelp.com/developers/documentation/v3/get_started and having a look for yourself!  

<img src="images/yelp_overview.png" width="800">

As you see at the top, the first piece of almost every API is authentication. 

This is where we started in the previous codealong, where we went to https://www.yelp.com/developers/v3/manage_app and created a new app. 

<img src="images/yelp_app.png"  width="800">

Let's take a closer look at Yelp's authentication documentation:  
https://www.yelp.com/developers/documentation/v3/authentication
    
<img src="images/yelp_auth.png" width="800">

Notice in the documentation, it gives us the specific format "Put the API Key in the request header as "Authorization: Bearer <YOUR API KEY>"." This is what we passed in our get request.   


Before we do this, let's store your authentication token which you used from the codealong before **in a separate file**.

#### Create a `.secret/` directory


```python
!mkdir .secret
```

#### Ensure it was created correctly


```python
!ls -a
```

#### Store your API key in a `.json` file

In the cell below, replace `<YOUR_API_KEY>` with the API key you were given by yelp. Once again, it is recommnended you delete this cell once you make sure your API key was stored and imported correctly


```python
!echo '{"api_key": "<YOUR_API_KEY>"}' >> yelp_api.json
```

#### Move `yelp_api.json` to `.secret/` directory


```python
!mv yelp_api.json .secret/
```


```python
import json

#Our previous function for loading our api key file
def get_keys(path):
    with open(path) as f:
        return json.load(f)
```

> **Note**: Like before, we have provided the file path to the `.json` file for you below



```python
keys = get_keys("./.secret/yelp_api.json")

api_key = keys['api_key']

#While you may wish to print out these api keys to check that they imported properly,
#be sure to clear the output before uploading to Github. 
#Again, you don't want your keys stolen!!!
```


```python
import requests
```


```python
url = #This will be our next step
header = {"Authorization" : "Bearer {}".format(api_key)}
response = requests.get(url, header=header)
```

 With that, let's take a look at how the rest of our request should be formatted. Go to https://www.yelp.com/developers/documentation/v3/business_search  and take a couple of minutes to look things over.
 
 <img src="images/yelp_docs.png"  width="800">

Notice the first part is the format of the get request! This is the URL we pass into our get request. From there, the available parameters that you can pass are listed. These define your query, some are required while others are optional.

Reviewing our python package we thus have:


```python
url = 'https://api.yelp.com/v3/businesses/search'

headers = {
        'Authorization': 'Bearer {}'.format(api_key),
    }

url_params = {
                'location': 'NYC'
            }
response = requests.get(url, headers=headers, params=url_params)
```

Note that location or latitude and longitude are the only required parameters. That said, you are free to pass as many parameters as you want such as:


```python
url = 'https://api.yelp.com/v3/businesses/search'

headers = {
        'Authorization': 'Bearer {}'.format(api_key),
    }

url_params = {
                'location': 'NYC',
                'term' : 'pizza',
                'limit' : 50,
                'price' : "1,2,3,4",
                'open_now' : True
            }
response = requests.get(url, headers=headers, params=url_params)
```

The final important note is how some of the parameters have alternative formats. For example, `limit` is an integer and `open_now` is a boolean according to the documentation. Following these conventions is essential to receiving valid responses.

# Summary

Congratulations! Not only have you seen an API now, you've also practiced sifting through the documentation. The last piece in working with APIs is developing a more solid understanding of JSON files; the data format typically returned by modern APIs. Take some additional time and familiarize yourself with some further aspects of the documentation which you may wish to investigate in the upcoming lab!
