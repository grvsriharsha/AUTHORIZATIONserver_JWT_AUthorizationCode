# AUTHORIZATIONserver_JWT_AUthorizationCode

This is stand alone AuthorizationServer.It does require Db Repo and userdetailservice.
It uses the private key and creates token and the creates signature.


#########Authorizationcode Flow######


AuthorizationCode work flow

Resource server/Client is  running on 9091

and autherzationserver running on 9092


 The Resourceserver/Client redirects the user to a url so he can enter Authorizationserver(like FB login details) login details.This is browser get call.

http://localhost:9092/oauth/authorize?response_type=code&client_id={clientId}&scope=read

Here clientid is 'couponclientapp'(not be confused with resourceId which links Authserver and ResourceServer)

Now the Authorization server sends its auth code to a redirect url that  Resourceserver/Client hosts.Lets say 
 Resourceserver/Client configured 'http://localhost:9091/codeHandlerPage'(RUNS THIS ON BROWSER TO GET THE CODE)

The authcode is avaibale at this redirectUrl,that resourceserver/Client collects it.

This code is now collected and Resourceserver/Client hit authorization server with this code to get the token

 'http://localhost:9092/oauth/token'
 with same postbody,but this time with grant_type as authorization_code and code is added in the post body.

Client recives the token and client extracts the user details and finally fetches the request,if his role permits.
The rolechecking happens in ResourceWebConfig only,but Authorizaton happens in AuthorizationServer.


HTTP REQUEST FOR TOKEN AFTER GETTING THE AUTH_CODE:

curl --location --request POST 'http://localhost:9092/oauth/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Authorization: Basic Y291cG9uY2xpZW50YXBwOjk5OTk=' \
--header 'Cookie: JSESSIONID=CC2B758F85008DA9A5E3DE9807AC463F' \
--data-urlencode 'grant_type=authorization_code' \
--data-urlencode 'scope=read' \
--data-urlencode 'code=X8bdGA'

