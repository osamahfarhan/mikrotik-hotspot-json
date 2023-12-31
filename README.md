# mikrotik-hotspot-json By Osamah Alhumaidi

### Features
- you can get or post json data from hotspot server
- login or logout with javascrpt
- get status info
- json parameters
```html
  {
    "idle_timeout": "$(idle-timeout)",
    "refresh_timeout": "$(refresh-timeout)",
    "uptime": "$(uptime)",
    "session_timeout": "$(session-timeout)",
    "session_time_left": "$(session-time-left)",
    "bytes_in_nice": "$(bytes-in-nice)",
    "bytes_out_nice": "$(bytes-out-nice)",
    "idle_timeout_secs": "$(idle-timeout-secs)",
    "refresh_timeout_secs": "$(refresh-timeout-secs)",
    "uptime_secs": "$(uptime-secs)",
    "limit_bytes_in": "$(limit-bytes-in)",
    "limit_bytes_out": "$(limit-bytes-out)",
    "limit_bytes_total": "$(limit-bytes-total)",
    "session_timeout_secs": "$(session-timeout-secs)",
    "session_time_left_secs": "$(session-time-left-secs)",
    "bytes_in": "$(bytes-in)",
    "bytes_out": "$(bytes-out)",
    "packets_in": "$(packets-in)",
    "packets_out": "$(packets-out)",
    "remain_bytes_in": "$(remain-bytes-in)",
    "remain_bytes_out": "$(remain-bytes-out)",
    "remain_bytes_total": "$(remain-bytes-total)",
    "login_by_mac": "$(login-by-mac)",
    "logged_in": "$(logged-in)",
    "erase_cookie":"$(erase-cookie)",
    "popup":"$(popup)",
    "advert_pending": "$(advert-pending)",
    "blocked": "$(blocked)",
    "trial": "$(trial)",
    "radius": "$(radius)",
    "plain_passwd": "$(plain-passwd)",
    "ssl_login": "$(ssl-login)",
    "chap_id": "$(chap-id-esc)",
    "chap_challenge": "$(chap-challenge-esc)",
    "error": "$(error-esc)",
    "error_orig": "$(error-orig-esc)",
    "error_type": "$(error-type-esc)",
    "session_id": "$(session-id)",
    "server_address": "$(server-address)",
    "vlan_id": "$(vlan-id)",
    "ip": "$(ip)",
    "host_ip": "$(host-ip)",
    "link_orig": "$(link-orig-esc)",
    "link_redirect": "$(link-redirect-esc)",
    "link_login": "$(link-login-esc)",
    "link_login_only": "$(link-login-only-esc)",
    "link_logout": "$(link-logout-esc)",
    "link_status": "$(link-status-esc)",
    "link_advert": "$(link-advert-esc)",
    "var": "$(var-esc)",
    "login_by": "$(login-by)",
    "mac": "$(mac)",
    "username": "$(username-esc)",
    "domain": "$(domain-esc)",
    "user_agent": "$(user-agent-esc)",
    "target_dir": "$(target-dir-esc)",
    "dst": "$(link-orig-esc)",
    "hostname": "$(hostname-esc)",
    "identity": "$(identity-esc)",
    "interface_name": "$(interface-name-esc)",
    "server_name": "$(server-name-esc)",
    "location_id": "$(location-id-esc)",
    "location_name": "$(location-name-esc)"
}
```
some params that has ***-esc***
you should use ***decodeURIComponent***  to decode them

### Getting started

Call the generic.js file from html 

```html
  <script src="/generic.js"></script>
```
### now you can use get post method 

to get json data you should pass param **var=json** in requested url EX: ***http://example.com/login?var=json***

using javascrpt 
```html
<script src="/generic.js"></script>
<script>
ajax.get('/',{'var' : 'json'},function(res){console.log(res);});
</script>
```


### get Json Data like chap-id and chap-challenge then login 

```html
<script src="/md5.js"></script>
<script src="/generic.js"></script>
<script>
var username = 'test';
var password = 'test';
function toJson(str) {
    let obj = { };
    try {
        obj = JSON.parse(decodeURIComponent(res));
        obj.chap_id = eval("'" + obj.chap_id + "'");
        obj.chap_challenge = eval("'" + obj.chap_challenge + "'");
    } catch (e) {console.error(e);}
    return obj;
}
ajax.get('/',{ 'var' : 'json' },function(res){
  let obj = toJson(res);
  if (obj.plain_passwd == 'yes' && obj.chap_id) {
    password =  hexMD5(obj.chap_id + password + obj.chap_challenge);
  }
  let LoginUrl =  obj.link_login_only || '/';
  ajax.post(LoginUrl,{ 'var' : 'json','username':username,'password':password } ,function(res){console.log(res);});
});
</script>
```
### build your Functions

```html
<script src="/md5.js"></script>
<script src="/generic.js"></script>
<script>
 async function getJson() {
    const response = await fetch('?var=json', { method: 'GET', headers: { "Content-Type": "application/json" } });
    const json = await response.json();
    return json;
}
function doLogin(username, password) {
    getJson().then((e) => {
        console.log(e);
        e.chap_id = eval("'" + decodeURIComponent(e.chap_id) + "'");
        e.chap_challenge = eval("'" + decodeURIComponent(e.chap_challenge) + "'");
        e.link_login_only = decodeURIComponent(e.link_login_only);
        if (e.plain_passwd == 'yes') {
            password = hexMD5(e.chap_id + password + e.chap_challenge);
        }
        let data = { 'username': username, 'password': password };
        let link_login_only = e.link_login_only || '/';
        ajax.post(link_login_only, { 'var': 'json', 'username': username, 'password': password }, function (res) { console.log(res); });
    })
}
doLogin('test','test');
</script>
```
### Enjoy (* @ *)


### Refrence
 mikrotik wiki : ***https://wiki.mikrotik.com/wiki/Manual:Customizing_Hotspot***

### contact 
Email: ***osamahfarhan1995@gmail.com***

Fb: ***http://fb.com/osamahfarhan***

