---
author: admin
comments: true
date: 2012-08-22 13:15:03+00:00
layout: post
slug: generating-c-classes-from-json
title: Generating C# Classes from JSON
wordpress_id: 1841
categories:
- JSON
- Tips
- Tools
---

I’ve long advocated using JSON when building mobile and cloud applications. If nothing else, the payload size makes it extremely efficient when transferred over the wire – take a look at the size of the same information formatted as OData, REST-XML, and lastly JSON:

![JSON versus OData versus REST-XML](https://wadewegner.blob.core.windows.net/wordpress/2012/08/JSON.png)

Pretty compelling.

Despite the use of JSON – and great frameworks like [JSON.NET](http://json.codeplex.com/) and [SimpleJson](https://github.com/facebook-csharp-sdk/simple-json) – I always struggled with creating my C# classes when working with an existing web service that returned JSON. It can take a long time to create these C# classes correctly, and often time I’d take a lazy approach and either use the JObject or an IDictionary such that I didn’t have to have a C# class – something like:

	var json = (IDictionary<string, object>)SimpleJson.DeserializeObject(data);

Yesterday I stumbled upon a tool that makes this SO amazingly easy. In many ways I’m bothered by the fact that it’s taken me so long to find it – has this been one of the best kept secrets on the Internet or did I just miss it?

[http://json2csharp.com/](http://json2csharp.com/)

This website is as simple as it is powerful. Simply paste your JSON into the textbox, click Generate, and voilà you have C# objects!

Take a look. Here’s some JSON returned back from the [Untappd API](http://untappd.com/api/):

	{
	   "meta":{
	      "code":200,
	      "responsetime":{
	         "time":0.109,
	         "measure":"seconds"
	      }
	   },
	   "notifications":[
	
	   ],
	   "response":{
	      "pagination":{
	         "nexturl":"http://api.untappd.com/v4/thepub?maxid=11697698",
	         "maxid":11697698,
	         "sinceurl":"http://api.untappd.com/v4/thepub?minid=11697724"
	      },
	      "checkins":{
	         "count":2,
	         "items":[
	            {
	               "checkinid":11697724,
	               "createdat":"Wed, 22 Aug 2012 12:56:41 +0000",
	               "checkincomment":"",
	               "user":{
	                  "uid":205218,
	                  "username":"asiahobo",
	                  "firstname":"Bum",
	                  "lastname":"",
	                  "location":"",
	                  "url":"0",
	                  "relationship":null,
	                  "bio":"0",
	                  "useravatar":"https://untappd.s3.amazonaws.com/profile/7d21ba831edb33341b98f86e09795ed7thumb.jpg",
	                  "contact":{
	                     "twitter":"asiahobo",
	                     "foursquare":31652652
	                  }
	               },
	               "beer":{
	                  "bid":9652,
	                  "beername":"Maredsous 8° Brune",
	                  "beerlabel":"https://untappd.s3.amazonaws.com/site/beerlogos/beer-maredsous.jpg",
	                  "beerstyle":"Belgian Dubbel",
	                  "authrating":0,
	                  "wishlist":false
	               },
	               "brewery":{
	                  "breweryid":6,
	                  "breweryname":"Abbaye de Maredsous (Duvel Moortgat)",
	                  "brewerylabel":"https://untappd.s3.amazonaws.com/site/brewerylogos/brewery-AbbayedeMaredsousDuvelMoortgat6.jpeg",
	                  "countryname":"Belgium",
	                  "contact":{
	                     "twitter":"",
	                     "facebook":"www.facebook.com/pages/Abbaye-De-Maredsous/208016262548587fine",
	                     "url":"www.maredsous.be/"
	                  },
	                  "location":{
	                     "brewery_city":"",
	                     "brewerystate":"Denée",
	                     "lat":50.3044,
	                     "lng":4.77149
	                  }
	               },
	               "venue":[
	
	               ],
	               "comments":{
	                  "count":0,
	                  "items":[
	
	                  ]
	               },
	               "toasts":{
	                  "count":0,
	                  "authtoast":null,
	                  "items":[
	
	                  ]
	               },
	               "media":{
	                  "count":0,
	                  "items":[
	
	                  ]
	               }
	            },
	            {
	               "checkinid":11697723,
	               "createdat":"Wed, 22 Aug 2012 12:56:35 +0000",
	               "checkincomment":"",
	               "user":{
	                  "uid":137722,
	                  "username":"Mjoepp",
	                  "firstname":"Christoffer",
	                  "lastname":"",
	                  "location":"Linköping",
	                  "url":"",
	                  "relationship":null,
	                  "bio":"",
	                  "useravatar":"http://gravatar.com/avatar/f1672535a7caa3bd686267257d33c588",
	                  "contact":{
	                     "foursquare":25958771
	                  }
	               },
	               "beer":{
	                  "bid":12145,
	                  "beername":"Chocolate",
	                  "beerlabel":"https://untappd.s3.amazonaws.com/site/beerlogos/beer-ChocolatePorter12145.jpeg",
	                  "beerstyle":"English Porter",
	                  "authrating":0,
	                  "wishlist":false
	               },
	               "brewery":{
	                  "breweryid":844,
	                  "breweryname":"Meantime Brewing Company",
	                  "brewerylabel":"https://untappd.s3.amazonaws.com/site/brewerylogos/brewery-MeantimeBrewingCompanyLimited844.jpeg",
	                  "countryname":"England",
	                  "contact":{
	                     "twitter":"MeantimeBrewing",
	                     "facebook":"http://www.facebook.com/meantimebrewing",
	                     "url":"http://www.meantimebrewing.com"
	                  },
	                  "location":{
	                     "brewerycity":"London",
	                     "brewerystate":"",
	                     "lat":51.5081,
	                     "lng":-0.128005
	                  }
	               },
	               "venue":[
	
	               ],
	               "comments":{
	                  "count":0,
	                  "items":[
	
	                  ]
	               },
	               "toasts":{
	                  "count":0,
	                  "authtoast":null,
	                  "items":[
	
	                  ]
	               },
	               "media":{
	                  "count":0,
	                  "items":[
	
	                  ]
	               }
	            }
	         ]
	      }
	   }
	}

I’m sad to admit that, in the past, I’d like create my C# objects by hand and then either conform to the JSON or map between the two. It requires a TON of time and is extremely error prone. With [http://json2csharp.com/](http://json2csharp.com/) all I do is paste this into the textbox and click Generate. I’ll get the following output:

	public class ResponseTime
	{
	    public double time { get; set; }
	    public string measure { get; set; }
	}
	public class Meta
	{
	    public int code { get; set; }
	    public ResponseTime response_time { get; set; }
	}
	public class Pagination
	{
	    public string nexturl { get; set; }
	    public int maxid { get; set; }
	    public string since_url { get; set; }
	}
	public class Contact
	{
	    public string twitter { get; set; }
	    public int foursquare { get; set; }
	}
	public class User
	{
	    public int uid { get; set; }
	    public string username { get; set; }
	    public string firstname { get; set; }
	    public string lastname { get; set; }
	    public string location { get; set; }
	    public string url { get; set; }
	    public object relationship { get; set; }
	    public string bio { get; set; }
	    public string useravatar { get; set; }
	    public Contact contact { get; set; }
	}
	public class Beer
	{
	    public int bid { get; set; }
	    public string beername { get; set; }
	    public string beerlabel { get; set; }
	    public string beer_style { get; set; }
	    public int authrating { get; set; }
	    public bool wishlist { get; set; }
	}
	public class Contact2
	{
	    public string twitter { get; set; }
	    public string facebook { get; set; }
	    public string url { get; set; }
	}
	public class Location
	{
	    public string brewerycity { get; set; }
	    public string brewerystate { get; set; }
	    public double lat { get; set; }
	    public double lng { get; set; }
	}
	public class Brewery
	{
	    public int breweryid { get; set; }
	    public string breweryname { get; set; }
	    public string brewerylabel { get; set; }
	    public string countryname { get; set; }
	    public Contact2 contact { get; set; }
	    public Location location { get; set; }
	}
	public class Comments
	{
	    public int count { get; set; }
	    public List<object> items { get; set; }
	}
	public class Toasts
	{
	    public int count { get; set; }
	    public object auth_toast { get; set; }
	    public List<object> items { get; set; }
	}
	public class Media
	{
	    public int count { get; set; }
	    public List<object> items { get; set; }
	}
	public class Item
	{
	    public int checkinid { get; set; }
	    public string createdat { get; set; }
	    public string checkin_comment { get; set; }
	    public User user { get; set; }
	    public Beer beer { get; set; }
	    public Brewery brewery { get; set; }
	    public List<object> venue { get; set; }
	    public Comments comments { get; set; }
	    public Toasts toasts { get; set; }
	    public Media media { get; set; }
	}
	public class Checkins
	{
	    public int count { get; set; }
	    public List items { get; set; }
	}
	public class Response
	{
	    public Pagination pagination { get; set; }
	    public Checkins checkins { get; set; }
	}
	public class RootObject
	{
	    public Meta meta { get; set; }
	    public List<object> notifications { get; set; }
	    public Response response { get; set; }
	}


Pretty amazing! Now, note that it’s not perfect – there’s both a Contact and Contact2 class, but that’s easy to fix by merging the two and updating references. I’ll gladly perform this little bit of cleanup given the hours this tool just saved me.

Now that I have these classes, it’s really easy to use JSON.NET to load them with data.

	RootObject publicFeed = new RootObject();

	using (StreamReader reader = new StreamReader(response.GetResponseStream()))
	{ 
		data = reader.ReadToEnd();
		publicFeed = JsonConvert.DeserializeObject<RootObject>(data); 
	}

Now it’s a simple matter of using my RootObject within my applications.

I feel like I may be the last person to have heard of this tool, in which case I’m both embarrassed and bitter – couldn’t you all have told me about this years ago? ![Smile](https://wadewegner.blob.core.windows.net/wordpress/2012/08/wlEmoticon-smile.png)

I hope this helps!