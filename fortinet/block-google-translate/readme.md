
# Block Google Translate from translating web pages

While Google Translate is a fantastic education tool it can also be used a proxy to circumnavigate web filtering policies.

I see this happen regularly in schools where web categories such as Pornography are blocked by the FortiGate web filter, but students will use Google Translate to get past this and view inappropriate web pages (albeit in a different language).

This post will go through how you can create a custom application signature to allow Google Translate to be used to translate words, but not used to translate/proxy web pages.

The first thing we'll need to do is create a custom application signature. On the FortiGate goto **Security Profiles > Custom Signature** and create a new application signature.

_____________________________________________________________

* Name the new signature _Google Translate Hyperlink_** **and copy/paste the following signature:


``` F-SBID( --name "Google.Translate.Hyperlink.Custom"; --protocol tcp; --service ssl; --pattern "|16 03|"; --context packet; --within 2,context; --byte_test 1,=,1,3,relative; --pattern "translate.googleusercontent.com"; --context packet; --weight 20; --app_cat 12; ) ```


Your new application should look like this:

![][1]

* Next goto **Security Profiles > Application Control** and edit the application sensor used for web traffic.

* Goto **Application Overrides** and add the Google.Translate.Hyperlink application and ensure the action is set to **Block**. Also ensure that QUIC is set to **Block. **Your application sensor should look like this:

![][2]

  
* Now it's time to add this application sensor to our internet policy. Goto **Policy & Objects > IPv4 Policy **and edit your policy to include the application sensor. Ensure that you also select **Deep Inspection** for the SSL/SSH Inspection profile.

![][3]

  
* Lastly it's time to test it out. Goto Google Translate and type in a url then select the hyperlink:   

![][4]

  
* You should now get the application blocked page:

![][5]

  
[1]: sig.png
[2]: app-sensor.png
[3]: policy.png
[4]: translate.png
[5]: blocked.png

  
