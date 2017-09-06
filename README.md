#
UCB Data Analytics Project

**Members: Neil, Song, Michael**

**Title: San Francisco Restaurant Business Analysis**

[Github Repository](https://github.com/nvodoor/RBA)

#Objective:  
We engaged in trying to find unique trends
in restaurant businneses within the districts of San
Francisco.  We concluded that pricing, ratings, total
restaurant counts, and ratings would provide the most 
meaningful data for our analysis.

#Implementation: 
In order to accomplish our task, we needed to use the Python programming language and multiple Python packages.  As of April, Yelp! has switched to their [Yelp! Fusion](https://www.yelp.com/fusion) package which seems to have complicated the use of their API services We began with Requests, Requests.oauthlib, BrowserClientWindow to extract data from the Yelp! Application Programming Interface.  This was used because the Yelp code examples to make calls to their API were overly complicated.  So we simplified the process by learning that since the Yelp API is a oauth v2 library that there is a way using requests.ouathlib and browserclientwindow to get our access token from Yelp.  We then used that method and then modified our request call accordingly so that we could more easily access data from the Yelp API.  An example code snippet can be observed below:

	import requests as req
	!pip install oauthlib
	!pip install requests_oauthlib
	from oauthlib.oauth2 import MobileApplicationClient
	from requests_oauthlib import OAuth2Session
	from oauthlib.oauth2 import BackendApplicationClient
	import json

	client_id = 'API information here'
	client_secret =	'API information here'

	client = BackendApplicationClient(client_id=client_id)
	oauth = OAuth2Session(client=client)
	token = oauth.fetch_token(token_url='https://	api.yelp.com/oauth2/token', client_id=client_id,
        client_secret=client_secret)

	print(token['access_token'])

	tok = token['access_token']

	url = "https://api.yelp.com/v3/businesses/search"

	headers = {
    'Authorization': 'Bearer %s' % tok,##
    
            url_params = {
            'term': y,
            'location': x,
            'limit': 50,
            'radius': 750
            
        }
        
        reviewcall = req.get(url, headers=headers, params=url_params)
        
        reviews = reviewcall.json()
        
        for x in districts:
    		for y in types:
        
                for rev in reviews['businesses']:
            try:
                if y not in types:
                    typeRating[y] = rev["rating"]
                    typeCount[y] = 1
                    typeReview[y] = rev["review_count"]
                    
                else:
                    typeRating[y] += rev["rating"]
                    typeCount[y] += 1
                    typeReview[y] += rev["review_count"]
            except:
                continue
                
                
                
This snippet will produce some results from the Yelp! Fusion API.

During class, we established lists of districts in San Francisco:

	districts = [
                "Castro District", "Chinatown", "Tenderloin", "Inner Richmond", 
                "Inner Sunset", "Alamo Square", "Russian Hill", "Mission", 
                "NorthBeach/Telegraph", "SoMa"
            ]
            
and cuisine types to explore:

	types = [
                "Chinese", "Mexican", "French", "Japanese", "Mediterranean", 
                "American", "Italian", "Korean", "Thai", "Indian"
            ]
            
From these lists we were able to generate dataframes using pandas package in Python.  From these dataframes, we could convert our values into visualizations using matplotlib package in Python.
            
[Example Bubble Plot](https://github.com/nvodoor/RBA/blob/master/TypesCount/ChinaTownCounts.png)

#Issues:
There were some issues that we found to be counter productitive during our project.  The biggest issue is that the Yelp! Fusion API search feature will only return 50 of the most relevant matches regardless of actual population size.  Because of this, we had to determine a radius search size that would accurately represent a district and still not cap out on search results.  We determined 750m would yield acceptable results without comprimising district representation.

There were Github repositories for API search code for the previous version of the API, however the new Fusion API version that is being used by Yelp! required a good deal of time by team members in order to utilize for this project.  

Our original intention was to implement crime data to see if there may be some correlation to the results that were generated.  However, there was not enough time to implement this additional layer of data analysis.  

#Challenges:
Due to the way the API is set up, only a maximum of 50 top searches will appear when a get request is made.  Because of this we needed to use the radius search parameter to try to get more representative results of restaurant distribution per each district.  We decided to limit the search radius to 750 meters in order to cover a large enough area and cover each district as best as possible.  

#Credits:

Thank you so much to everyone that contributed to this project.  It ended up being a larger challenge than originally anticipated.  



