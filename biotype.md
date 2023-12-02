---
description: >-
  WebDNA's approach towards user validation and authentication incorporates a
  series of biometric security algorithms that record and measure how a person
  uniquely types their credentials. The multi-dim
---

# BIOTYPE

### \[biotype] WebDNA v8.5+ [tag](https://docs.webdna.us/contexts-and-tags)

The beauty of WebDNA \[biotype]'s keystroke dynamics software lies in adding an extra layer of protection that is highly cost effective, accurate and user friendly. No additional hardware is needed, data are collected from users without being intrusive.

\[biotype] is a behavioral biometrics WebDNA function based on ADGS research and development and available on WebDNA from version 8.5.

\[biotype] is based on the concept of behavioral biometrics, meaning the way people do things individually, such as speaking, writing, typing, walking styles.

With \[biotype], WebDNA improves the way users are authenticated, ensuring imposters masquerading as the valid user are unable to access private or corporate information.

JavaScript is used to capture the keystroke from a web form that send the data to WebDNA. The \[biotype] function builds a profile for the user and stores it into a database.

**\[biotype] has 3 methods**

| Name       | Description                                                                                                                                                                                                                                                                                                                                             |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| initialize | ecords the keystroke dynamic for a specific user. He will have to type his credentials from 2 to 4 times to get enough data to build the profile.                                                                                                                                                                                                       |
| evaluate   | <p>Once the profile is built and stored for a user, [evaluate] will compare every new credential typing and return a value from 0.1 to 4.0<br>0.1 to 0.5 is an evaluation that can be translated as "almost certainly the same user" (>90% chances)<br>0.5 to 0.8 is probably the same user (80% to 90%)<br>0.8 to 4.0 is most probably an impostor</p> |
| train      | may be used by the administrator from time to time to rebuild the profile, if the initial conditions change: different keyboard layout, health or physical problems etc.                                                                                                                                                                                |

By passing the user name and the method initialize along with the captured data,\
WebDNA will create a profile for the user. The method evaluate can then return a deviation \[btuser\_deviation].

| Parameter     | Description                                                                                                                                                                                                       |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| method        | initialize/train/evaluate                                                                                                                                                                                         |
| capture       | Variable for the KSD data from javascript                                                                                                                                                                         |
| btuser        | Name of the \[biotype] user, ignored for demo                                                                                                                                                                     |
| bttype        | Set to "TEXT" for large blocks of text, otherwise defaults to password. This controls how \[biotype] evaluates the keystrokes                                                                                     |
| btcorrections | Maximum number of corrections allowed, defaults to 1                                                                                                                                                              |
| btlength      | Number of keystrokes for the text/password, defaults to 8                                                                                                                                                         |
| btthreshold   | This parameter allows to specify the \[user\_deviation] value for which a user will be considered "impostor". Anything above it triggers the imposter. Below is "legitimate". The result is shown with \[biotype] |

When running "evaluate", if the result is "LEGITIMATE" then afterwards \[biotype] will train with the data: this means that the data will automatically and transparently be used to train the system for this user. If the result is "IMPOSTER", then it will not train or modify the database for the user.

\[biotype] with Free Text\
A future version of WebDNA will also integrate a different keystroke dynamics service for free text. This method is "not what you type, but how you type". Accurate recognition of free text keystroke dynamics is challenging due to the unstructured and sparse nature of the data and its underlying variability.

In this approach, the user types in text, as usual, without any kind of extra work to be done for authentication. Moreover, it only involves the user's own keyboard and no other external hardware.

Free-text methodology does not restrict users to a particular text; on the contrary, a user is given complete freedom to use any text of any length without any constraints. \[biotype] will continue to collect the keystrokes, after successfully passing the identification session, throughout the whole time that the user is logged-in. The user's typing pattern is typically monitored during several days where he/she is performing regular typing tasks such as writing e-mails or typing word documents. The enrolment phase is transparent to the user.

Evaluation and Results of the Free Text \[biotype] method, by ADGS :\
The methods used by \[biotype] for WebDNA are based on reasearch and developments from ADGS. A dataset containing more than 20,000 sessions from 20 different users was used to evaluate the performance of free text keystroke dynamics authentication method. The sessions were captured during a one year period to reflect long term variations in typing rhythms. The user's moved between different computer terminals and keyboards and typed under varying conditions of tiredness and stress.

As usual with soft biometric authentication methods the error rates are not single fixed numbers but random variables, not only considering a set of users but even when considering a single user, because of the variations shown in biometric templates and sometimes in the classification methods.

The performance of the authentication method is measured using two error rate metrics: False Acceptance Rate (probability that an impostor is accepted as a legitimate user) and False Rejection Rate (probability that the legitimate user is rejected by the system)

An effective keystroke is an alphanumerical or space keystroke, thus excluding modifiers, functions, special and navigation keys. A session is a sequence of keystrokes typed by a single user in the normal course of his daily work, possibly including pauses and interruptions, both naturally occurring and external, including at least 100 effective keystrokes; the user might have left the computer terminal and resumed writing, as long as the interval in a single session remains under three minutes.

Fig 1\
![](https://docs.webdna.us/assets/images/biotype/FIG1FARSessions.jpg)\
Fig 2\
![](https://docs.webdna.us/assets/images/biotype/FIG2FRRSessions.jpg)\
Fig 3\
![](https://docs.webdna.us/assets/images/biotype/FIG3BestWorstUsersSessions.jpg)\
Fig 4\
![](https://docs.webdna.us/assets/images/biotype/FIG4BestWorstUsersEffectiveKeystrokes.jpg)\
Fig 5\
![](https://docs.webdna.us/assets/images/biotype/FIG5FARSingleUserVariation.jpg)\
Fig 6\
![](https://docs.webdna.us/assets/images/biotype/FIG6FRRSingleUserVariation.jpg)

In figures 1 and 2 the evolution of FAR and FRR as a function of the number of available training sessions is shown; the plot depicts average, one standard deviation channels, maximum and minimum. Figures 3 and 4 show the error rate evolution for the best and worst users in the data, as a function of the number of training sessions and of the effective keystroke count. Figures 5 and 6 show the variation of the FAR and FRR ranges for a single user with a high variability in error rate if different training sessions are used.

The asymptotical average error rates are 0.92% for FAR (with a standard deviation of 0.74) and 4.28% for FRR (with a standard deviation of 2.73); both distributions are positively skewed. Almost optimal error rates are achieved approximately after 50 sessions or 15 thousand effective keystrokes and error rates double that figure are achieved after half the sessions or effective keystroke count.

**\[biotype] demo included with WebDNA 8.2**

Here is the JavaScript code that captures the keystroke dynamics of a demo user and how to make WebDNA analyze it\
**Example code:**\
This example file is available to download here [KSD.html.zip](https://docs.webdna.us/files/KSD.html.zip)\
You will need one html form, named "KSD.html", containing all the JavaScript code, and one very simple return page.

```
<html>
  <head>
    <title>Keystroke capture demo</title>
    <script>
    function KeystrokeDynamicsCapture ( targetControlId )
    {
      vk_arr = [];
      ht_arr = [];
       ft_arr = [];       pendingKeyDown = {};
       lastKeyDown = undefined;       this.clear = function ()
       {
        vk_arr = [];
        ht_arr = [];
        ft_arr = [];        pendingKeyDown = {};
        lastKeyDown = undefined;
       }       var BACKSPACE = 8;
       this.serializeKeystrokeData = function ()
       {
        final_size = 0;
        final = [];        var retval = '';
        for ( i = 0 ; i < ht_arr.length ; i++ )
             if ( vk_arr[i] == BACKSPACE )
             {
              if ( final_size != 0 )
                final_size--;
             }
             else
             {
              final[final_size] = i;
              final_size++;
             }        for ( i = 0 ; i < final_size ; i++ )
        {
             var pos = final[i];
             retval += vk_arr[pos] + ':' + ht_arr[pos] + '/' + ft_arr[pos] + '-';
        }        return retval;
       }       function printDebug(str)
       {
        var dbg = document.getElementById("debug");
        dbg.innerHTML = str + "<br>" + dbg.innerHTML;
       }       function formatLists()
       {
        var retval = '[';
        for ( i = 0 ; i < ht_arr.length ; i++ )
             retval += '(' + vk_arr[i] + '/' + ht_arr[i] + '-' + ft_arr[i] + ')';        return retval + ']';
       }       var last_key_is_backspace = false;
       function onKeyDown(e)
       {
        var d = new Date();        vk_arr.push(e.keyCode);
        if ( lastKeyDown == undefined || last_key_is_backspace )
             ft_arr.push(0);
        else
        {
             var ft = d.getTime() - lastKeyDown;             ft_arr.push(ft);
             printDebug('KEY ' + e.keyCode + ' FT ' + ft + '  ' + formatLists());
        }        lastKeyDown = d.getTime();
        if ( pendingKeyDown[e.keyCode] == undefined )
             pendingKeyDown[e.keyCode] = d.getTime();        last_key_is_backspace = e.keyCode == BACKSPACE;
       }       function onKeyUp(e)
       {
        var d = new Date();
        if ( pendingKeyDown[e.keyCode] != undefined )
        {
             var ht = d.getTime() - pendingKeyDown[e.keyCode];
             ht_arr.push(ht);
             pendingKeyDown[e.keyCode] = undefined;
             printDebug('KEY ' + e.keyCode + ' HT ' + ht + '  ' + formatLists());
        }
       }       function registerHandlers(targetControlId)
       {
        var ctrl = document.getElementById(targetControlId);
        ctrl.addEventListener("keydown", onKeyDown);
        ctrl.addEventListener("keyup", onKeyUp);
       }       registerHandlers(targetControlId);
          }          var ksdc;
          function registerHandlers(targetControlId)
          {
       ksdc = new KeystrokeDynamicsCapture(targetControlId);
          }          function getKeystrokeDynamicsData()
          {
       // alert(ksdc.serializeKeystrokeData());
       document.getElementById("result").innerHTML += ksdc.serializeKeystrokeData() + "|";
       ksdc.clear();
          }          function sendKeystrokeDynamicsData()
          {
       form = document.getElementById('myTestForm');       // Delete old form data if it exists
       oldelem = document.getElementById('captureid');
       if (oldelem) form.removeChild(oldelem);       myvar = document.createElement('input');
       myvar.setAttribute('name', 'capture');
       myvar.setAttribute('type', 'hidden');
       myvar.setAttribute('id', 'captureid');
       myvar.setAttribute('value', ksdc.serializeKeystrokeData());
       form.appendChild(myvar);
       form.submit();
    }
  </script>
 </head>
 <body onload="registerHandlers('targetControl');">
  <form action="ksdtest.dna" method=POST id='myTestForm'>
    <input type="text" name="targetControl" id="targetControl" value="" size="64" maxlength="128" />
    <br>
    METHOD:<br>
    <input type="radio" name="method" value="initialize">Initialize<br>
    <input type="radio" name="method" value="train">Train<br>
    <input type="radio" name="method" value="evaluate" checked="checked">Evaluate<br>
    BTTHRESHOLD: <input type="text" name="btthreshold" value="0.69" size=3><br>
    BTCORRECTIONS: <input type="text" name="btcorrections" value="1" size=3><br>
    BTLENGTH: <input type="text" name="btlength" value="8" size=3><br>
    BTUSER: <input type="text" name="btuser" value="demouser" size=8><br>
    BTTYPE: <input type="radio" name="bttype" value="password">Password <input type="radio" name="bttype" value="TEXT" checked="checked">Text<br>
    <input type="button" onclick="sendKeystrokeDynamicsData();" value="Submit data">
  </form>  <hr />  <div id="debug" /> </body>
</html>
```

Start by typing an 8 characters password, using method "Initialize": this will open and store the user profile in a database in your WebDNA folder. At the same time, you will dynamically visualize the keystroke dynamics capture. Submit, then type and submit the same password two or three more time, in a consistent way, using the "Train" method. You are now ready to test. The treshold has been set at 0.69: depending on this value, the next page will show "LEGITIMATE" or "IMPOSTOR"

Results:
