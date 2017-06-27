##Google+三方登录注册与配置##
1. 登录到 Google API [https://console.developers.google.com/apis/library?project=sharp-weft-168607](https://console.developers.google.com/apis/library?project=sharp-weft-168607)
2. 进入 `社交API`　--> `Google+ API` --> `Credentials/凭证`

**Google+ 测试配置信息**
**客户端 ID:** `539270208723-8pagi39gg0nh6qv49mol2c2clo2lp81f.apps.googleusercontent.com`
**客户端密钥:** `4-IuXNGhlPU4mk1pcEQWTQOq`


##Documents##
[https://developers.google.com/+/web/api/rest/](https://developers.google.com/+/web/api/rest/)
[Set up Google Sign-In for Google+](https://developers.google.com/+/web/signin/)
[https://console.developers.google.com/apis/credentials?project=sharp-weft-168607](https://console.developers.google.com/apis/credentials?project=sharp-weft-168607)
[javascript DEMO](https://developers.google.com/identity/sign-in/web/build-button)
[SDK](https://developers.google.com/identity/protocols/OpenIDConnect#createxsrftoken)
[https://code.google.com/archive/p/google-api-php-client/downloads](https://code.google.com/archive/p/google-api-php-client/downloads)

**HTML DEMO**
```html
<html>
<head>
  <link href="https://fonts.googleapis.com/css?family=Roboto" rel="stylesheet" type="text/css">
  <script src="https://apis.google.com/js/api:client.js"></script>
  <script>
  var googleUser = {};
  var startApp = function() {
    gapi.load('auth2', function(){
      // Retrieve the singleton for the GoogleAuth library and set up the client.
      auth2 = gapi.auth2.init({
        client_id: '539270208723-8pagi39gg0nh6qv49mol2c2clo2lp81f.apps.googleusercontent.com',
        cookiepolicy: 'single_host_origin',
        // Request scopes in addition to 'profile' and 'email'
        //scope: 'additional_scope'
      });
      attachSignin(document.getElementById('customBtn'));
    });
  };

  function attachSignin(element) {
    console.log(element.id);
    auth2.attachClickHandler(element, {},
        function(googleUser) {
          document.getElementById('name').innerText = "Signed in: " +
              googleUser.getBasicProfile().getName();
        }, function(error) {
          alert(JSON.stringify(error, undefined, 2));
        });
  }
  </script>
  <style type="text/css">
    #customBtn {
      display: inline-block;
      background: white;
      color: #444;
      width: 190px;
      border-radius: 5px;
      border: thin solid #888;
      box-shadow: 1px 1px 1px grey;
      white-space: nowrap;
    }
    #customBtn:hover {
      cursor: pointer;
    }
    span.label {
      font-family: serif;
      font-weight: normal;
    }
    span.icon {
      background: url('/identity/sign-in/g-normal.png') transparent 5px 50% no-repeat;
      display: inline-block;
      vertical-align: middle;
      width: 42px;
      height: 42px;
    }
    span.buttonText {
      display: inline-block;
      vertical-align: middle;
      padding-left: 42px;
      padding-right: 42px;
      font-size: 14px;
      font-weight: bold;
      /* Use the Roboto font that is loaded in the <head> */
      font-family: 'Roboto', sans-serif;
    }
  </style>
  </head>
  <body>
  <!-- In the callback, you would hide the gSignInWrapper element on a
  successful sign in -->
  <div id="gSignInWrapper">
    <span class="label">Sign in with:</span>
    <div id="customBtn" class="customGPlusSignIn">
      <span class="icon"></span>
      <span class="buttonText">Google</span>
    </div>
  </div>
  <div id="name"></div>
  <script>startApp();</script>
</body>
</html>
```