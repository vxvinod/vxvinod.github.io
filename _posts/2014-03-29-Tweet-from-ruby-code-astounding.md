---
layout: post
title:  "Tweet from ruby code astounding"
date:   2014-03-29 
desc: "Today I decided to explore how to use twitter api in ruby and result was mind blowing this is the first time i am using this to tweet the status.
and i have noted some important not about REST .
"
---

Today I decided to explore how to use twitter api in ruby and result was mind blowing this is the first time i am using this to tweet the status.
and i have noted some important not about REST .

####What is api ?

application program interface specifies the ways where program can interact to application.

we user REST full API to communicate 

* GET: retrieves information from the specified source.
* POST: sends new information to the specified source.
* PUT: updates existing information of the specified source.
+ DELETE: removes existing  information from the specified source 
 
 #### HTTP Request contains three parts 
  1.request line - tells GET/POST.

  2.Header- sends additional information which client is making request.

  3.body - for GET it is empty and for POST it contains data to be posted.

  *API grants access to use particular API and keepr track of how their service is use to avoid maicious activity.

  * so me API require athentication  using a protocol called OAUTH.

  *successful request to server results in response 

  ####response from the server contains 3 digits status code.

  1xx-working  on request.

  2xx-successfully responding to your request.

  3xx-can do but require rerouting kind of activi ty .

  4xx- mistake source not found.

  5xx-server goofed up cant respon d now.


  #### HTTP Response also contains three parts.
  1- response line (3 digits status code)
  2-header-info abt server and its response.
  3-body-text of response.
  

  ##HOW TO USE TWITTER API

  1.create a new consumer instance by passing Twitter Api key and secret key in a configuration hash.

  2. Usually we used consumer key to request token and redirect to authorise url.

  Start the process by requesting a token

  #### @request_token = @consumer.get_request_token
  #### session[:request_token] = @request_token
  #### redirect_to @request_token.authorize_url
  
When user requests to create an access_token

3. but here in twitter ,itself provides access token and access security token.

  * so create a new access_token instance by passing request token and access secret token in configuration hash.

4. Next step to frame the URL to get data from the twitter.

* set baseurl
* set path according to post or get data from twitter.
* query - pass the hash in which screen to fetch the tweet and count 10.

5. URI provides classes to handle Uniform Resource Indentifiers.

6.  set up GET request using passing the URI

#### NET::HTTP::GET.new /1/1statuses/.....


7. SET UP HTTP REQUEST
 
  * initialise http using host and port number.
  * http.use_ssl =true for https request.
  * in order to have secured  SSL connnection server and client needs to verify the peer which is done by
#### http.verify_mode = OpenSSL::SSL::VERIFY_PEER

8. Now next step is to issue request.

  * before issueing the request we need to add OAUTH information to http request

  #### request.oauth! http, consumer_key, access_token

9.We need to provide a HTTP Request that is done by 

#### http.start.
#### NET::HTTP.new address.host,address.port
#### response = http.request request

10. now the final step get the response and parse the response and print the tweet these all to be done only if response.code='200' that server has successfully responded the request.

tweet = nil
if response.code == '200' then
  tweets = JSON.parse(response.body)
  #puts tweets.to_yaml
  tweets.each do |tweet|
    puts tweet["text"]
  end
  puts "Successfully tweets fetched"
else
  puts "Could not send the Tweet! " +
  "Code:#{response.code} Body:#{response.body}"
end




GET TWEET FROM USER ACCOUNT

{%highlight ruby%}
require 'rubygems'
require 'oauth'
require 'json'
require 'yaml'

# You will need to set your application type to
# read/write on dev.twitter.com and regenerate your access
# token.  Enter the new values here:
consumer_key = OAuth::Consumer.new(
  "dt3IDYtpiyj0FhcGjXjkkg",
  "vkeuYgZJc1X9wyFENJ4PnkrDz62MCP331iAsV4SIyrE")
access_token = OAuth::Token.new(
  "1684808822-cGuMIjScxtk7Ot99Odd97q832nKZBAhUIF7Izsa",
  "xcWPDHDRxE8ijDGrLQf1lMnNRRExmEwhpiQCOaQFTFIFo")

# Note that the type of request has changed to POST.
# The request parameters have also moved to the body
# of the request instead of being put in the URL.
baseurl = "https://api.twitter.com"
path    = "/1.1/statuses/user_timeline.json"
query   = URI.encode_www_form(
    "screen_name" => "theRealKiyosaki",
    "count" => 10,
)
address = URI("#{baseurl}#{path}?#{query}")


puts address
puts "request uri  "+address.request_uri


# Set up HTTP.
http             = Net::HTTP.new address.host, address.port
http.use_ssl     = true
http.verify_mode = OpenSSL::SSL::VERIFY_PEER
request = Net::HTTP::Get.new address.request_uri

# Issue the request.
request.oauth! http, consumer_key, access_token
http.start
response = http.request request

# Parse and print the Tweet if the response code was 200
tweet = nil
if response.code == '200' then
  tweets = JSON.parse(response.body)
  #puts tweets.to_yaml
  tweets.each do |tweet|
    puts tweet["text"]
  end
  puts "Successfully tweets fetched"
else
  puts "Could not send the Tweet! " +
  "Code:#{response.code} Body:#{response.body}"
end

{%endhighlight%}