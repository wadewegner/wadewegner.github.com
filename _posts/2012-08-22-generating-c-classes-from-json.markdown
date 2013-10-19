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

 

![JSON versus OData versus REST-XML](http://images.wadewegner.com/wordpress/2012/08/JSON.png)

 

Pretty compelling.

 

Despite the use of JSON – and great frameworks like [JSON.NET](http://json.codeplex.com/) and [SimpleJson](https://github.com/facebook-csharp-sdk/simple-json) – I always struggled with creating my C# classes when working with an existing web service that returned JSON. It can take a long time to create these C# classes correctly, and often time I’d take a lazy approach and either use the JObject or an IDictionary such that I didn’t have to have a C# class – something like:

 
    
    <span style="color: blue">var </span>json = (<span style="color: #2b91af">IDictionary</span><<span style="color: blue">string</span>, <span style="color: blue">object</span>>)<span style="color: #2b91af">SimpleJson</span>.DeserializeObject(data);





Yesterday I stumbled upon a tool that makes this SO amazingly easy. In many ways I’m bothered by the fact that it’s taken me so long to find it – has this been one of the best kept secrets on the Internet or did I just miss it?





[http://json2csharp.com/](http://json2csharp.com/)





This website is as simple as it is powerful. Simply paste your JSON into the textbox, click Generate, and voilà you have C# objects!





Take a look. Here’s some JSON returned back from the [Untappd API](http://untappd.com/api/):




    
    <span style="background: white; color: black">{
      </span><span style="background: white; color: #a31515">"meta"</span><span style="background: white; color: black">: {
        </span><span style="background: white; color: #a31515">"code"</span><span style="background: white; color: black">: 200,
        </span><span style="background: white; color: #a31515">"response_time"</span><span style="background: white; color: black">: {
          </span><span style="background: white; color: #a31515">"time"</span><span style="background: white; color: black">: 0.109,
          </span><span style="background: white; color: #a31515">"measure"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"seconds"
        </span><span style="background: white; color: black">}
      },
      </span><span style="background: white; color: #a31515">"notifications"</span><span style="background: white; color: black">: [],
      </span><span style="background: white; color: #a31515">"response"</span><span style="background: white; color: black">: {
        </span><span style="background: white; color: #a31515">"pagination"</span><span style="background: white; color: black">: {
          </span><span style="background: white; color: #a31515">"next_url"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"http://api.untappd.com/v4/thepub?max_id=11697698"</span><span style="background: white; color: black">,
          </span><span style="background: white; color: #a31515">"max_id"</span><span style="background: white; color: black">: 11697698,
          </span><span style="background: white; color: #a31515">"since_url"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"http://api.untappd.com/v4/thepub?min_id=11697724"
        </span><span style="background: white; color: black">},
        </span><span style="background: white; color: #a31515">"checkins"</span><span style="background: white; color: black">: {
          </span><span style="background: white; color: #a31515">"count"</span><span style="background: white; color: black">: 2,
          </span><span style="background: white; color: #a31515">"items"</span><span style="background: white; color: black">: [
            {
              </span><span style="background: white; color: #a31515">"checkin_id"</span><span style="background: white; color: black">: 11697724,
              </span><span style="background: white; color: #a31515">"created_at"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Wed, 22 Aug 2012 12:56:41 +0000"</span><span style="background: white; color: black">,
              </span><span style="background: white; color: #a31515">"checkin_comment"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">""</span><span style="background: white; color: black">,
              </span><span style="background: white; color: #a31515">"user"</span><span style="background: white; color: black">: {
                </span><span style="background: white; color: #a31515">"uid"</span><span style="background: white; color: black">: 205218,
                </span><span style="background: white; color: #a31515">"user_name"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"asiahobo"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"first_name"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Bum"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"last_name"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">""</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"location"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">""</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"url"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"0"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"relationship"</span><span style="background: white; color: black">: </span><span style="background: white; color: blue">null</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"bio"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"0"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"user_avatar"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"https://untappd.s3.amazonaws.com/profile/7d21ba831edb33341b98f86e09795ed7_thumb.jpg"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"contact"</span><span style="background: white; color: black">: {
                  </span><span style="background: white; color: #a31515">"twitter"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"asiahobo"</span><span style="background: white; color: black">,
                  </span><span style="background: white; color: #a31515">"foursquare"</span><span style="background: white; color: black">: 31652652
                }
              },
              </span><span style="background: white; color: #a31515">"beer"</span><span style="background: white; color: black">: {
                </span><span style="background: white; color: #a31515">"bid"</span><span style="background: white; color: black">: 9652,
                </span><span style="background: white; color: #a31515">"beer_name"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Maredsous 8° Brune"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"beer_label"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"https://untappd.s3.amazonaws.com/site/beer_logos/beer-maredsous.jpg"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"beer_style"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Belgian Dubbel"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"auth_rating"</span><span style="background: white; color: black">: 0,
                </span><span style="background: white; color: #a31515">"wish_list"</span><span style="background: white; color: black">: </span><span style="background: white; color: blue">false
              </span><span style="background: white; color: black">},
              </span><span style="background: white; color: #a31515">"brewery"</span><span style="background: white; color: black">: {
                </span><span style="background: white; color: #a31515">"brewery_id"</span><span style="background: white; color: black">: 6,
                </span><span style="background: white; color: #a31515">"brewery_name"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Abbaye de Maredsous (Duvel Moortgat)"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"brewery_label"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"https://untappd.s3.amazonaws.com/site/brewery_logos/brewery-AbbayedeMaredsousDuvelMoortgat_6.jpeg"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"country_name"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Belgium"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"contact"</span><span style="background: white; color: black">: {
                  </span><span style="background: white; color: #a31515">"twitter"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">""</span><span style="background: white; color: black">,
                  </span><span style="background: white; color: #a31515">"facebook"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"www.facebook.com/pages/Abbaye-De-Maredsous/208016262548587fine"</span><span style="background: white; color: black">,
                  </span><span style="background: white; color: #a31515">"url"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"www.maredsous.be/"
                </span><span style="background: white; color: black">},
                </span><span style="background: white; color: #a31515">"location"</span><span style="background: white; color: black">: {
                  </span><span style="background: white; color: #a31515">"brewery_city"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">""</span><span style="background: white; color: black">,
                  </span><span style="background: white; color: #a31515">"brewery_state"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Denée"</span><span style="background: white; color: black">,
                  </span><span style="background: white; color: #a31515">"lat"</span><span style="background: white; color: black">: 50.3044,
                  </span><span style="background: white; color: #a31515">"lng"</span><span style="background: white; color: black">: 4.77149
                }
              },
              </span><span style="background: white; color: #a31515">"venue"</span><span style="background: white; color: black">: [],
              </span><span style="background: white; color: #a31515">"comments"</span><span style="background: white; color: black">: {
                </span><span style="background: white; color: #a31515">"count"</span><span style="background: white; color: black">: 0,
                </span><span style="background: white; color: #a31515">"items"</span><span style="background: white; color: black">: []
              },
              </span><span style="background: white; color: #a31515">"toasts"</span><span style="background: white; color: black">: {
                </span><span style="background: white; color: #a31515">"count"</span><span style="background: white; color: black">: 0,
                </span><span style="background: white; color: #a31515">"auth_toast"</span><span style="background: white; color: black">: </span><span style="background: white; color: blue">null</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"items"</span><span style="background: white; color: black">: []
              },
              </span><span style="background: white; color: #a31515">"media"</span><span style="background: white; color: black">: {
                </span><span style="background: white; color: #a31515">"count"</span><span style="background: white; color: black">: 0,
                </span><span style="background: white; color: #a31515">"items"</span><span style="background: white; color: black">: []
              }
            },
            {
              </span><span style="background: white; color: #a31515">"checkin_id"</span><span style="background: white; color: black">: 11697723,
              </span><span style="background: white; color: #a31515">"created_at"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Wed, 22 Aug 2012 12:56:35 +0000"</span><span style="background: white; color: black">,
              </span><span style="background: white; color: #a31515">"checkin_comment"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">""</span><span style="background: white; color: black">,
              </span><span style="background: white; color: #a31515">"user"</span><span style="background: white; color: black">: {
                </span><span style="background: white; color: #a31515">"uid"</span><span style="background: white; color: black">: 137722,
                </span><span style="background: white; color: #a31515">"user_name"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Mjoepp"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"first_name"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Christoffer"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"last_name"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">""</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"location"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Linköping"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"url"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">""</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"relationship"</span><span style="background: white; color: black">: </span><span style="background: white; color: blue">null</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"bio"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">""</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"user_avatar"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"http://gravatar.com/avatar/f1672535a7caa3bd686267257d33c588?size=100&d=https%3A%2F%2Funtappd.s3.amazonaws.com%2Fsite%2Fassets%2Fimages%2Fdefault_avatar.jpg"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"contact"</span><span style="background: white; color: black">: {
                  </span><span style="background: white; color: #a31515">"foursquare"</span><span style="background: white; color: black">: 25958771
                }
              },
              </span><span style="background: white; color: #a31515">"beer"</span><span style="background: white; color: black">: {
                </span><span style="background: white; color: #a31515">"bid"</span><span style="background: white; color: black">: 12145,
                </span><span style="background: white; color: #a31515">"beer_name"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Chocolate"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"beer_label"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"https://untappd.s3.amazonaws.com/site/beer_logos/beer-ChocolatePorter_12145.jpeg"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"beer_style"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"English Porter"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"auth_rating"</span><span style="background: white; color: black">: 0,
                </span><span style="background: white; color: #a31515">"wish_list"</span><span style="background: white; color: black">: </span><span style="background: white; color: blue">false
              </span><span style="background: white; color: black">},
              </span><span style="background: white; color: #a31515">"brewery"</span><span style="background: white; color: black">: {
                </span><span style="background: white; color: #a31515">"brewery_id"</span><span style="background: white; color: black">: 844,
                </span><span style="background: white; color: #a31515">"brewery_name"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"Meantime Brewing Company"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"brewery_label"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"https://untappd.s3.amazonaws.com/site/brewery_logos/brewery-MeantimeBrewingCompanyLimited_844.jpeg"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"country_name"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"England"</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"contact"</span><span style="background: white; color: black">: {
                  </span><span style="background: white; color: #a31515">"twitter"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"MeantimeBrewing"</span><span style="background: white; color: black">,
                  </span><span style="background: white; color: #a31515">"facebook"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"http://www.facebook.com/meantimebrewing"</span><span style="background: white; color: black">,
                  </span><span style="background: white; color: #a31515">"url"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"http://www.meantimebrewing.com"
                </span><span style="background: white; color: black">},
                </span><span style="background: white; color: #a31515">"location"</span><span style="background: white; color: black">: {
                  </span><span style="background: white; color: #a31515">"brewery_city"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">"London"</span><span style="background: white; color: black">,
                  </span><span style="background: white; color: #a31515">"brewery_state"</span><span style="background: white; color: black">: </span><span style="background: white; color: #a31515">""</span><span style="background: white; color: black">,
                  </span><span style="background: white; color: #a31515">"lat"</span><span style="background: white; color: black">: 51.5081,
                  </span><span style="background: white; color: #a31515">"lng"</span><span style="background: white; color: black">: -0.128005
                }
              },
              </span><span style="background: white; color: #a31515">"venue"</span><span style="background: white; color: black">: [],
              </span><span style="background: white; color: #a31515">"comments"</span><span style="background: white; color: black">: {
                </span><span style="background: white; color: #a31515">"count"</span><span style="background: white; color: black">: 0,
                </span><span style="background: white; color: #a31515">"items"</span><span style="background: white; color: black">: []
              },
              </span><span style="background: white; color: #a31515">"toasts"</span><span style="background: white; color: black">: {
                </span><span style="background: white; color: #a31515">"count"</span><span style="background: white; color: black">: 0,
                </span><span style="background: white; color: #a31515">"auth_toast"</span><span style="background: white; color: black">: </span><span style="background: white; color: blue">null</span><span style="background: white; color: black">,
                </span><span style="background: white; color: #a31515">"items"</span><span style="background: white; color: black">: []
              },
              </span><span style="background: white; color: #a31515">"media"</span><span style="background: white; color: black">: {
                </span><span style="background: white; color: #a31515">"count"</span><span style="background: white; color: black">: 0,
                </span><span style="background: white; color: #a31515">"items"</span><span style="background: white; color: black">: []
              }
            }
          ]
        }
      }
    }</span>





I’m sad to admit that, in the past, I’d like create my C# objects by hand and then either conform to the JSON or map between the two. It requires a TON of time and is extremely error prone. With [http://json2csharp.com/](http://json2csharp.com/) all I do is paste this into the textbox and click Generate. I’ll get the following output:




    
    <span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">ResponseTime
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public double </span><span style="background: white; color: black">time { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">measure { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Meta
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public int </span><span style="background: white; color: black">code { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">ResponseTime response_time { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Pagination
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">next_url { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public int </span><span style="background: white; color: black">max_id { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">since_url { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Contact
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">twitter { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public int </span><span style="background: white; color: black">foursquare { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">User
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public int </span><span style="background: white; color: black">uid { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">user_name { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">first_name { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">last_name { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">location { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">url { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public object </span><span style="background: white; color: black">relationship { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">bio { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">user_avatar { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">Contact contact { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Beer
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public int </span><span style="background: white; color: black">bid { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">beer_name { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">beer_label { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">beer_style { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public int </span><span style="background: white; color: black">auth_rating { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public bool </span><span style="background: white; color: black">wish_list { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Contact2
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">twitter { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">facebook { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">url { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Location
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">brewery_city { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">brewery_state { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public double </span><span style="background: white; color: black">lat { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public double </span><span style="background: white; color: black">lng { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Brewery
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public int </span><span style="background: white; color: black">brewery_id { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">brewery_name { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">brewery_label { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">country_name { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">Contact2 contact { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">Location location { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Comments
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public int </span><span style="background: white; color: black">count { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">List<</span><span style="background: white; color: blue">object</span><span style="background: white; color: black">> items { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Toasts
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public int </span><span style="background: white; color: black">count { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public object </span><span style="background: white; color: black">auth_toast { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">List<</span><span style="background: white; color: blue">object</span><span style="background: white; color: black">> items { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Media
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public int </span><span style="background: white; color: black">count { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">List<</span><span style="background: white; color: blue">object</span><span style="background: white; color: black">> items { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Item
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public int </span><span style="background: white; color: black">checkin_id { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">created_at { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public string </span><span style="background: white; color: black">checkin_comment { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">User user { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">Beer beer { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">Brewery brewery { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">List<</span><span style="background: white; color: blue">object</span><span style="background: white; color: black">> venue { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">Comments comments { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">Toasts toasts { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">Media media { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Checkins
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public int </span><span style="background: white; color: black">count { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">List<Item> items { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">Response
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">Pagination pagination { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">Checkins checkins { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }
    
    </span><span style="background: white; color: blue">public class </span><span style="background: white; color: #2b91af">RootObject
    </span><span style="background: white; color: black">{
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">Meta meta { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">List<</span><span style="background: white; color: blue">object</span><span style="background: white; color: black">> notifications { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
        </span><span style="background: white; color: blue">public </span><span style="background: white; color: black">Response response { </span><span style="background: white; color: blue">get</span><span style="background: white; color: black">; </span><span style="background: white; color: blue">set</span><span style="background: white; color: black">; }
    }</span>





Pretty amazing! Now, note that it’s not perfect – there’s both a Contact and Contact2 class, but that’s easy to fix by merging the two and updating references. I’ll gladly perform this little bit of cleanup given the hours this tool just saved me.





Now that I have these classes, it’s really easy to use JSON.NET to load them with data.




    
    <span style="background: white; color: #2b91af">RootObject </span><span style="background: white; color: black">publicFeed = </span><span style="background: white; color: blue">new </span><span style="background: white; color: #2b91af">RootObject</span><span style="background: white; color: black">();
    
    </span><span style="background: white; color: blue">using </span><span style="background: white; color: black">(</span><span style="background: white; color: #2b91af">StreamReader </span><span style="background: white; color: black">reader = </span><span style="background: white; color: blue">new </span><span style="background: white; color: #2b91af">StreamReader</span><span style="background: white; color: black">(response.GetResponseStream()))
    {
        data = reader.ReadToEnd();
    
        publicFeed = </span><span style="background: white; color: #2b91af">JsonConvert</span><span style="background: white; color: black">.DeserializeObject<</span><span style="background: white; color: #2b91af">RootObject</span><span style="background: white; color: black">>(data);
    }</span>





Now it’s a simple matter of using my RootObject within my applications.





I feel like I may be the last person to have heard of this tool, in which case I’m both embarrassed and bitter – couldn’t you all have told me about this years ago? ![Smile](http://images.wadewegner.com/wordpress/2012/08/wlEmoticon-smile.png)





I hope this helps!
