# Unit 4 Lab - Bastions

## Jailing a User
### Follow the lab here answering the questions below as you progress: https://killercoda.com/het-tanis/course/Linux-Labs/204-building-a-chroot-jail
```
ubuntu:~$ echo "Installing scenario..."
Installing scenario...
ubuntu:~$ while [ ! -f /tmp/finished ]; do sleep 1; done
ubuntu:~$ echo DONE
DONE
ubuntu:~$ mkdir /var/chroot
ubuntu:~$ mkdir -p /var/chroot/{bin,lib64,dev,etc,home,usr/bin,lib/x86_64-linux-gnu}
ubuntu:~$ ls -l /var/chroot
total 28
drwxr-xr-x 2 root root 4096 May 24 16:47 bin
drwxr-xr-x 2 root root 4096 May 24 16:47 dev
drwxr-xr-x 2 root root 4096 May 24 16:47 etc
drwxr-xr-x 2 root root 4096 May 24 16:47 home
drwxr-xr-x 3 root root 4096 May 24 16:47 lib
drwxr-xr-x 2 root root 4096 May 24 16:47 lib64
drwxr-xr-x 3 root root 4096 May 24 16:47 usr
ubuntu:~$ cp /usr/bin/bash /var/chroot/bin/bash
ubuntu:~$ for package in $(ldd /bin/bash | awk '{print $(NF -1)}'); do cp $package /var/chroot/$package; done
cp: cannot stat 'linux-vdso.so.1': No such file or directory
ubuntu:~$ echo $$
1884
ubuntu:~$ chroot /var/chroot
ubuntu:/$ echo $$
2083
ubuntu:/$ ls                                                                           
bash: ls: command not found
ubuntu:/$ top                                                                          
bash: top: command not found
ubuntu:/$ bash                                                                         
ubuntu:/$ exit
ubuntu:/$ ps -ef                                                                       
bash: ps: command not found
ubuntu:/$ exit                                                                         
exit
ubuntu:~$ ping -c 1 www.google.com
PING www.google.com (142.251.167.104) 56(84) bytes of data.
64 bytes from www.google.com (142.251.167.104): icmp_seq=1 ttl=105 time=18.6 ms

--- www.google.com ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 18.610/18.610/18.610/0.000 ms
ubuntu:~$ for file in passwd group nsswitch.conf hosts; do cp /etc/$file /var/chroot/etc/$file; done
ubuntu:~$ for binary in bash ssh curl; do cp /usr/bin/$binary /var/chroot/usr/bin/$binary; done
ubuntu:~$ ldd /usr/bin/bash
        linux-vdso.so.1 (0x00007ffe19df0000)
        libtinfo.so.6 => /lib/x86_64-linux-gnu/libtinfo.so.6 (0x0000745d443bc000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x0000745d44000000)
        /lib64/ld-linux-x86-64.so.2 (0x0000745d44565000)
ubuntu:~$ for package in $(ldd /usr/bin/ssh | awk '{print $(NF -1)}'); do cp $package /var/chroot/$package; done
cp: cannot stat 'linux-vdso.so.1': No such file or directory
ubuntu:~$ for package in $(ldd /usr/bin/curl | awk '{print $(NF -1)}'); do cp $package /var/chroot/$package; done
cp: cannot stat 'linux-vdso.so.1': No such file or directory
ubuntu:~$ mknod -m 666 /var/chroot/dev/null c 1 3
ubuntu:~$ mknod -m 666 /var/chroot/dev/tty c 5 0
ubuntu:~$ mknod -m 666 /var/chroot/dev/zero c 1 5
ubuntu:~$ mknod -m 666 /var/chroot/dev/random c 1 8
ubuntu:~$ mknod -m 666 /var/chroot/dev/urandom c 1 9
ubuntu:~$ cp -r /lib/x86_64-linux-gnu/*nss* /var/chroot/lib/x86_64-linux-gnu/
ubuntu:~$ useradd -m jailed
ubuntu:~$ passwd jailed
New password: 
Retype new password: 
passwd: password updated successfully
ubuntu:~$ chroot /var/chroot
ubuntu:/$ curl www.google.com
<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="en"><head><meta content="Search the world's information, including webpages, images, videos and more. Google has many special features to help you find exactly what you're looking for." name="description"><meta content="noodp, " name="robots"><meta content="text/html; charset=UTF-8" http-equiv="Content-Type"><meta content="/images/branding/googleg/1x/googleg_standard_color_128dp.png" itemprop="image"><title>Google</title><script nonce="44vA003SFjojLDzSV_4vpw">(function(){var _g={kEI:'qvkxaM_HObOg5NoPquX8oAw',kEXPI:'0,202792,25,23,14,2,610014,2887417,1097,538661,48791,46127,78218,266578,247320,42724,5241682,36812642,25228681,123988,14280,14115,57126,5381,2655,3439,3319,23879,9138,4600,328,6225,24366,29843,9246,710,6748,8300,49,8162,3286,4134,12136,18244,28333,10904,31985,5327,80,1811,2848,1257,353,11781,7099,74,2,5794,3097,4610,7,5773,4311,9173,2819,349,10960,5556,6607,4361,3261,2990,28,7,1,1293,2126,2863,1,726,1765,2,2589,888,1443,3206,570,1791,4355,5392,656,5027,3604,8523,2886,1114,1,623,3,4615,7,1,2676,53,860,1442,2875,2,1253,114,1005,1606,841,2,3573,2,1,2,2,2,3,219,2098,649,908,2808,506,1,4,3554,1257,334,41,1547,486,1,2782,2829,276,5,636,3,1,1,1,5,234,1574,883,593,622,1,756,2526,181,2,82,1216,1223,537,4,1289,328,890,365,211,25,874,158,1579,4851,1,671,175,253,1607,3,159,3,274,2345,821,1051,2386,424,220,230,211,1110,2,3,6,3,437,86,12,5,4,1,54,287,6,410,26,758,28,705,831,3,644,357,329,652,330,238,14,130,50,95,49,88,6,34,57,18,630,374,535,12,2,678,7,155,9,1597,2,111,815,244,369,1,3,114,321,9,3,203,578,160,394,249,1302,348,288,137,181,774,136,827,3,2,2,2,21,3,49,77,592,589,218,39,3,1,208,181,628,483,227,494,66,3,30,75,328,4,1,1,1,331,3,23,102,217,1,2,6,228,5,6,800,62,2,121,16,4,112,67,445,232,98,123,87,38,13,286,263,280,164,1,476,161,310,520,781,5,22,125,1193,1177,1,12,1,270,657,1160,2355,21267309,1717,1580,3651,6,2398,5,2992,4,2806,154,3,3918,3,97,315,15,2329,207,1607,2,1071,453,212,419,14,301,509,729,8497352,28046,110865',kBL:'iaK4',kOPI:89978449};(function(){var a;((a=window.google)==null?0:a.stvsc)?google.kEI=_g.kEI:window.google=_g;}).call(this);})();(function(){google.sn='webhp';google.kHL='en';})();(function(){
var g=this||self;function k(){return window.google&&window.google.kOPI||null};var l,m=[];function n(a){for(var b;a&&(!a.getAttribute||!(b=a.getAttribute("eid")));)a=a.parentNode;return b||l}function p(a){for(var b=null;a&&(!a.getAttribute||!(b=a.getAttribute("leid")));)a=a.parentNode;return b}function q(a){/^http:/i.test(a)&&window.location.protocol==="https:"&&(google.ml&&google.ml(Error("a"),!1,{src:a,glmm:1}),a="");return a}
function r(a,b,d,c,h){var e="";b.search("&ei=")===-1&&(e="&ei="+n(c),b.search("&lei=")===-1&&(c=p(c))&&(e+="&lei="+c));var f=b.search("&cshid=")===-1&&a!=="slh";c="&zx="+Date.now().toString();g._cshid&&f&&(c+="&cshid="+g._cshid);(d=d())&&(c+="&opi="+d);return"/"+(h||"gen_204")+"?atyp=i&ct="+String(a)+"&cad="+(b+e+c)};l=google.kEI;google.getEI=n;google.getLEI=p;google.ml=function(){return null};google.log=function(a,b,d,c,h,e){e=e===void 0?k:e;d||(d=r(a,b,e,c,h));if(d=q(d)){a=new Image;var f=m.length;m[f]=a;a.onerror=a.onload=a.onabort=function(){delete m[f]};a.src=d}};google.logUrl=function(a,b){b=b===void 0?k:b;return r("",a,b)};}).call(this);(function(){google.y={};google.sy=[];var d;(d=google).x||(d.x=function(a,b){if(a)var c=a.id;else{do c=Math.random();while(google.y[c])}google.y[c]=[a,b];return!1});var e;(e=google).sx||(e.sx=function(a){google.sy.push(a)});google.lm=[];var f;(f=google).plm||(f.plm=function(a){google.lm.push.apply(google.lm,a)});google.lq=[];var g;(g=google).load||(g.load=function(a,b,c){google.lq.push([[a],b,c])});var h;(h=google).loadAll||(h.loadAll=function(a,b){google.lq.push([a,b])});google.bx=!1;var k;(k=google).lx||(k.lx=function(){});var l=[],m;(m=google).fce||(m.fce=function(a,b,c,n){l.push([a,b,c,n])});google.qce=l;}).call(this);google.f={};(function(){
document.documentElement.addEventListener("submit",function(b){var a;if(a=b.target){var c=a.getAttribute("data-submitfalse");a=c==="1"||c==="q"&&!a.elements.q.value?!0:!1}else a=!1;a&&(b.preventDefault(),b.stopPropagation())},!0);document.documentElement.addEventListener("click",function(b){var a;a:{for(a=b.target;a&&a!==document.documentElement;a=a.parentElement)if(a.tagName==="A"){a=a.getAttribute("data-nohref")==="1";break a}a=!1}a&&b.preventDefault()},!0);}).call(this);</script><style>#gbar,#guser{font-size:13px;padding-top:1px !important;}#gbar{height:22px}#guser{padding-bottom:7px !important;text-align:right}.gbh,.gbd{border-top:1px solid #c9d7f1;font-size:1px}.gbh{height:0;position:absolute;top:24px;width:100%}@media all{.gb1{height:22px;margin-right:.5em;vertical-align:top}#gbar{float:left}}a.gb1,a.gb4{text-decoration:underline !important}a.gb1,a.gb4{color:#00c !important}.gbi .gb4{color:#dd8e27 !important}.gbf .gb4{color:#900 !important}
</style><style>body,td,a,p,.h{font-family:sans-serif}body{margin:0;overflow-y:scroll}#gog{padding:3px 8px 0}td{line-height:.8em}.gac_m td{line-height:17px}form{margin-bottom:20px}.h{color:#1967d2}em{font-weight:bold;font-style:normal}.lst{height:25px;width:496px}.gsfi,.lst{font:18px sans-serif}.gsfs{font:17px sans-serif}.ds{display:inline-box;display:inline-block;margin:3px 0 4px;margin-left:4px}input{font-family:inherit}body{background:#fff;color:#000}a{color:#681da8;text-decoration:none}a:hover,a:active{text-decoration:underline}.fl a{color:#1967d2}a:visited{color:#681da8}.sblc{padding-top:5px}.sblc a{display:block;margin:2px 0;margin-left:13px;font-size:11px}.lsbb{background:#f8f9fa;border:solid 1px;border-color:#dadce0 #70757a #70757a #dadce0;height:30px}.lsbb{display:block}#WqQANb a{display:inline-block;margin:0 12px}.lsb{background:url(/images/nav_logo229.png) 0 -261px repeat-x;color:#000;border:none;cursor:pointer;height:30px;margin:0;outline:0;font:15px sans-serif;vertical-align:top}.lsb:active{background:#dadce0}.lst:focus{outline:none}</style><script nonce="44vA003SFjojLDzSV_4vpw">(function(){window.google.erd={jsr:1,bv:2226,de:true,dpf:'b9ABJT_Qle3lZSu3Lz7gnqLjjUwLmbLugWmeHt3LsOA'};
var g=this||self;var k,l=(k=g.mei)!=null?k:1,m,p=(m=g.diel)!=null?m:0,q,r=(q=g.sdo)!=null?q:!0,t=0,u,w=google.erd,x=w.jsr;google.ml=function(a,b,d,n,e){e=e===void 0?2:e;b&&(u=a&&a.message);d===void 0&&(d={});d.cad="ple_"+google.ple+".aple_"+google.aple;if(google.dl)return google.dl(a,e,d,!0),null;b=d;if(x<0){window.console&&console.error(a,b);if(x===-2)throw a;b=!1}else b=!a||!a.message||a.message==="Error loading script"||t>=l&&!n?!1:!0;if(!b)return null;t++;d=d||{};b=encodeURIComponent;var c="/gen_204?atyp=i&ei="+b(google.kEI);google.kEXPI&&(c+="&jexpid="+b(google.kEXPI));c+="&srcpg="+b(google.sn)+"&jsr="+b(w.jsr)+
"&bver="+b(w.bv);w.dpf&&(c+="&dpf="+b(w.dpf));var f=a.lineNumber;f!==void 0&&(c+="&line="+f);var h=a.fileName;h&&(h.indexOf("-extension:/")>0&&(e=3),c+="&script="+b(h),f&&h===window.location.href&&(f=document.documentElement.outerHTML.split("\n")[f],c+="&cad="+b(f?f.substring(0,300):"No script found.")));google.ple&&google.ple===1&&(e=2);c+="&jsel="+e;for(var v in d)c+="&",c+=b(v),c+="=",c+=b(d[v]);c=c+"&emsg="+b(a.name+": "+a.message);c=c+"&jsst="+b(a.stack||"N/A");c.length>=12288&&(c=c.substr(0,12288));a=c;n||google.log(0,"",a);return a};window.onerror=function(a,b,d,n,e){u!==a&&(a=e instanceof Error?e:Error(a),d===void 0||"lineNumber"in a||(a.lineNumber=d),b===void 0||"fileName"in a||(a.fileName=b),google.ml(a,!1,void 0,!1,a.name==="SyntaxError"||a.message.substring(0,11)==="SyntaxError"||a.message.indexOf("Script error")!==-1?3:p));u=null;r&&t>=l&&(window.onerror=null)};})();</script></head><body bgcolor="#fff"><script nonce="44vA003SFjojLDzSV_4vpw">(function(){var src='/images/nav_logo229.png';var iesg=false;document.body.onload = function(){window.n && window.n();if (document.images){new Image().src=src;}
if (!iesg){document.f&&document.f.q.focus();document.gbqf&&document.gbqf.q.focus();}
}
})();</script><div id="mngb"><div id=gbar><nobr><b class=gb1>Search</b> <a class=gb1 href="https://www.google.com/imghp?hl=en&tab=wi">Images</a> <a class=gb1 href="http://maps.google.com/maps?hl=en&tab=wl">Maps</a> <a class=gb1 href="https://play.google.com/?hl=en&tab=w8">Play</a> <a class=gb1 href="https://www.youtube.com/?tab=w1">YouTube</a> <a class=gb1 href="https://news.google.com/?tab=wn">News</a> <a class=gb1 href="https://mail.google.com/mail/?tab=wm">Gmail</a> <a class=gb1 href="https://drive.google.com/?tab=wo">Drive</a> <a class=gb1 style="text-decoration:none" href="https://www.google.com/intl/en/about/products?tab=wh"><u>More</u> &raquo;</a></nobr></div><div id=guser width=100%><nobr><span id=gbn class=gbi></span><span id=gbf class=gbf></span><span id=gbe></span><a href="http://www.google.com/history/optout?hl=en" class=gb4>Web History</a> | <a  href="/preferences?hl=en" class=gb4>Settings</a> | <a target=_top id=gb_70 href="https://accounts.google.com/ServiceLogin?hl=en&passive=true&continue=http://www.google.com/&ec=GAZAAQ" class=gb4>Sign in</a></nobr></div><div class=gbh style=left:0></div><div class=gbh style=right:0></div></div><center><br clear="all" id="lgpd"><div id="XjhHGf"><img alt="Google" height="92" src="/images/branding/googlelogo/1x/googlelogo_white_background_color_272x92dp.png" style="padding:28px 0 14px" width="272" id="hplogo"><br><br></div><form action="/search" name="f"><table cellpadding="0" cellspacing="0"><tr valign="top"><td width="25%">&nbsp;</td><td align="center" nowrap=""><input name="ie" value="ISO-8859-1" type="hidden"><input value="en" name="hl" type="hidden"><input name="source" type="hidden" value="hp"><input name="biw" type="hidden"><input name="bih" type="hidden"><div class="ds" style="height:32px;margin:4px 0"><input class="lst" style="margin:0;padding:5px 8px 0 6px;vertical-align:top;color:#000" autocomplete="off" value="" title="Google Search" maxlength="2048" name="q" size="57"></div><br style="line-height:0"><span class="ds"><span class="lsbb"><input class="lsb" value="Google Search" name="btnG" type="submit"></span></span><span class="ds"><span class="lsbb"><input class="lsb" id="tsuid_qvkxaM_HObOg5NoPquX8oAw_1" value="I'm Feeling Lucky" name="btnI" type="submit"><script nonce="44vA003SFjojLDzSV_4vpw">(function(){var id='tsuid_qvkxaM_HObOg5NoPquX8oAw_1';document.getElementById(id).onclick = function(){if (this.form.q.value){this.checked = 1;if (this.form.iflsig)this.form.iflsig.disabled = false;}
else top.location='/doodles/';};})();</script><input value="AOw8s4IAAAAAaDIHukyzBt_TkcldmeWCvNGZ-Ub5Nw2c" name="iflsig" type="hidden"></span></span></td><td class="fl sblc" align="left" nowrap="" width="25%"><a href="/advanced_search?hl=en&amp;authuser=0">Advanced search</a></td></tr></table><input id="gbv" name="gbv" type="hidden" value="1"><script nonce="44vA003SFjojLDzSV_4vpw">(function(){var a,b="1";if(document&&document.getElementById)if(typeof XMLHttpRequest!="undefined")b="2";else if(typeof ActiveXObject!="undefined"){var c,d,e=["MSXML2.XMLHTTP.6.0","MSXML2.XMLHTTP.3.0","MSXML2.XMLHTTP","Microsoft.XMLHTTP"];for(c=0;d=e[c++];)try{new ActiveXObject(d),b="2"}catch(h){}}a=b;if(a=="2"&&location.search.indexOf("&gbv=2")==-1){var f=google.gbvu,g=document.getElementById("gbv");g&&(g.value=a);f&&window.setTimeout(function(){location.href=f},0)};}).call(this);</script></form><div style="font-size:83%;min-height:3.5em"><br></div><span id="footer"><div style="font-size:10pt"><div style="margin:19px auto;text-align:center" id="WqQANb"><a href="/intl/en/ads/">Advertising</a><a href="/services/">Business Solutions</a><a href="/intl/en/about.html">About Google</a></div></div><p style="font-size:8pt;color:#5e5e5e">&copy; 2025 - <a href="/intl/en/policies/privacy/">Privacy</a> - <a href="/intl/en/policies/terms/">Terms</a></p></span></center><script nonce="44vA003SFjojLDzSV_4vpw">(function(){window.google.cdo={height:757,width:1440};(function(){var a=window.innerWidth,b=window.innerHeight;if(!a||!b){var c=window.document,d=c.compatMode=="CSS1Compat"?c.documentElement:c.body;a=d.clientWidth;b=d.clientHeight}if(a&&b&&(a!=google.cdo.width||b!=google.cdo.height)){var e=google,f=e.log,g="/client_204?&atyp=i&biw="+a+"&bih="+b+"&ei="+google.kEI,h="",k=window.google&&window.google.kOPI||null;k&&(h+="&opi="+k);f.call(e,"","",g+h)};}).call(this);})();(function(){google.xjs={basecomb:'/xjs/_/js/k\x3dxjs.hp.en.8TaJkayy2q0.es5.O/ck\x3dxjs.hp.Bgng2IOHX70.L.X.O/am\x3dAAAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAJSAQRAAAAAIgAEAAAAAAABABACIAAAIAAAAACURAMCBAADQAgCQIABygNIDAAACgAAAAAAAIAAAAAgIAIAAAACAAAAAAAyA8DgmAAAAEAALAAIAAACIRw/d\x3d1/ed\x3d1/dg\x3d0/ujg\x3d1/rs\x3dACT90oFVbZjkzyw5LxZw6kl-5l5VIv7qHw?cb\x3d72947180',basecss:'/xjs/_/ss/k\x3dxjs.hp.Bgng2IOHX70.L.X.O/am\x3dAAAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAJSAARAAAAAIAAAAAAAAAAAABACAAAAIAAAAACUBAAAAAADQAgCQIABygNIDAAACgAAAAAAAIAAAAAgIAIAAAACAAAAAAAAAAAgG/rs\x3dACT90oEBUUFVfNKSurOmYSWAnstYDk0Qbw?cb\x3d72947180',basejs:'/xjs/_/js/k\x3dxjs.hp.en.8TaJkayy2q0.es5.O/am\x3dAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAAAAgAEAAAAAAABABACIAAAAAAAAAAAQAMCBAACQAgAAAABwAAAAAAAAgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAyA8DgmAAAAEAALAAIAAACIRw/dg\x3d0/rs\x3dACT90oGb7gzO7wkRM6g_Xy1vv17G66t4vg?cb\x3d72947180',excm:[]};})();(function(){var u='/xjs/_/js/k\x3dxjs.hp.en.8TaJkayy2q0.es5.O/am\x3dAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAAAAgAEAAAAAAABABACIAAAAAAAAAAAQAMCBAACQAgAAAABwAAAAAAAAgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAyA8DgmAAAAEAALAAIAAACIRw/d\x3d1/ed\x3d1/dg\x3d3/rs\x3dACT90oGb7gzO7wkRM6g_Xy1vv17G66t4vg/m\x3dsb_he,d?cb\x3d72947180';var st=1;var amd=1000;var mmd=0;var pod=true;var pop=true;var povp=false;var fp='';
var e=this||self;function f(){var b,a,d;if(a=b=(a=window.google)==null?void 0:(d=a.ia)==null?void 0:d.r.B2Jtyd)a=b.m,a=a===1||a===5;return a&&b.cbfd!=null&&b.cbvi!=null?b:void 0};function g(){var b=[u];if(!google.dp){for(var a=0;a<b.length;a++){var d=b[a],c=document.createElement("link");c.as="script";c.href=d;c.rel="preload";document.body.appendChild(c)}google.dp=!0}};google.ps===void 0&&(google.ps=[]);function h(){var b=u,a=function(){};google.lx=google.stvsc?a:function(){k(b);google.lx=a};google.bx||google.lx()}function l(b,a){a&&(b.src=a);fp&&(b.fetchPriority=fp);var d=b.onload;b.onload=function(c){d&&d(c);google.ps=google.ps.filter(function(G){return b!==G})};google.ps.push(b);document.body.appendChild(b)}google.as=l;function k(b){google.timers&&google.timers.load&&google.tick&&google.tick("load","xjsls");var a=document.createElement("script");a.onerror=function(){google.ple=1};a.onload=function(){google.ple=0};google.xjsus=void 0;l(a,b);google.aple=-1;google.dp=!0};function m(b){var a=b.getAttribute("jscontroller");return(a==="UBXHI"||a==="R3fhkb"||a==="TSZEqd")&&b.hasAttribute("data-src")}function n(){for(var b=document.getElementsByTagName("img"),a=0,d=b.length;a<d;a++){var c=b[a];if(c.hasAttribute("data-lzy_")&&Number(c.getAttribute("data-atf"))&1&&!m(c))return!0}return!1}for(var p=document.getElementsByTagName("img"),q=0,r=p.length;q<r;++q){var t=p[q];Number(t.getAttribute("data-atf"))&1&&m(t)&&(t.src=t.getAttribute("data-src"))};var w,x,y,z,A,B,C,D,E,F;function H(){google.xjsu=u;e._F_jsUrl=u;A=function(){h()};w=!1;x=(st===1||st===3)&&!!google.caft&&!n();y=f();z=(st===2||st===3)&&!!y&&!n();B=pod;C=pop;D=povp;E=pop&&document.prerendering||povp&&document.hidden;F=D?"visibilitychange":"prerenderingchange"}function I(){w||x||z||E||(A(),w=!0)}
setTimeout(function(){google&&google.tick&&google.timers&&google.timers.load&&google.tick("load","xjspls");H();if(x||z||E){if(x){var b=function(){x=!1;I()};google.caft(b);window.setTimeout(b,amd)}z&&(b=function(){z=!1;I()},y.cbvi.push(b),window.setTimeout(b,mmd));if(E){var a=function(){(D?document.hidden:document.prerendering)||(E=!1,I(),document.removeEventListener(F,a))};document.addEventListener(F,a,{passive:!0})}if(B||C||D)w||g()}else A()},0);})();window._ = window._ || {};window._DumpException = _._DumpException = function(e){throw e;};window._s = window._s || {};_s._DumpException = _._DumpException;window._qs = window._qs || {};_qs._DumpException = _._DumpException;(function(){var t=[0,16384,0,0,0,0,0,539295744,4161,100671488,20971520,268435456,8912901,680699392,441450502,544276484,49283081,8537088,1038616352,142606336,132,8388608,8388608,2097154,8454144,805306368,596576256,9,722960,8,292992];window._F_toggles = window._xjs_toggles = t;})();window._F_installCss = window._F_installCss || function(css){};(function(){google.jl={bfl:0,dw:false,eli:false,ine:false,ubm:false,uwp:true,vs:false};})();(function(){var pmc='{\x22d\x22:{},\x22sb_he\x22:{\x22client\x22:\x22heirloom-hp\x22,\x22dh\x22:true,\x22ds\x22:\x22\x22,\x22host\x22:\x22google.com\x22,\x22jsonp\x22:true,\x22msgs\x22:{\x22cibl\x22:\x22Clear Search\x22,\x22dym\x22:\x22Did you mean:\x22,\x22lcky\x22:\x22I\\u0026#39;m Feeling Lucky\x22,\x22lml\x22:\x22Learn more\x22,\x22psrc\x22:\x22This search was removed from your \\u003Ca href\x3d\\\x22/history\\\x22\\u003EWeb History\\u003C/a\\u003E\x22,\x22psrl\x22:\x22Remove\x22,\x22sbit\x22:\x22Search by image\x22,\x22srch\x22:\x22Google Search\x22},\x22ovr\x22:{},\x22pq\x22:\x22\x22,\x22rfs\x22:[],\x22stok\x22:\x22U8F1exZclN3NrlBE_h2xTGkpqFw\x22}}';google.pmc=JSON.parse(pmc);})();</script></body></html>ubuntu:/$    ubuntu:/$ exit                                                            
exit
ubuntu:~$ chroot /var/chroot
ubuntu:/$ ssh -l jailed 127.0.0.1
The authenticity of host '127.0.0.1 (127.0.0.1)' can't be established.
ED25519 key fingerprint is SHA256:XikjTSvie4xfSHl8CD14UKeq6b3m5zuJAlVVC6JLlao.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/root/.ssh' (No such file or directory).
Failed to add the host to the list of known hosts (/root/.ssh/known_hosts).
jailed@127.0.0.1's password: 
$ id
uid=1001(jailed) gid=1001(jailed) groups=1001(jailed)
$ pwd
/home/jailed
$ curl www.google.com
<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="en"><head><meta content="Search the world's information, including webpages, images, videos and more. Google has many special features to help you find exactly what you're looking for." name="description"><meta content="noodp, " name="robots"><meta content="text/html; charset=UTF-8" http-equiv="Content-Type"><meta content="/images/branding/googleg/1x/googleg_standard_color_128dp.png" itemprop="image"><title>Google</title><script nonce="py6ph9lqL06-MzScU1lnBA">(function(){var _g={kEI:'5vkxaLvGJK6g5NoP_smwuAc',kEXPI:'0,202791,26,22,15,2,3497475,108,510,435,538661,14111,34680,390923,100066,147253,42725,5230281,11400,36812643,25228681,112222,11766,14280,14115,8939,53568,2655,3439,3319,23879,7033,2106,4599,328,6225,64165,15048,50,8161,3286,4134,30380,28333,10904,31986,7217,4105,353,5000,2,1,6,13871,5870,1898,1202,4607,7,5773,1202,3108,12342,8298,1,3,2658,12163,4361,3261,2990,28,7,1,1293,535,1591,2863,1,5081,889,4649,570,1791,459,1346,596,1951,5395,658,5025,3605,8522,463,2423,1110,1,627,3,2859,221,1535,7,1,2729,628,221,892,560,3149,981,712,408,1606,943,3473,2,1,2,2,2,3,2316,280,370,4222,1,4,3558,972,281,334,41,560,3,695,290,485,1,2782,2578,149,2,58,41,1163,2457,199,393,560,63,1,751,2530,182,2,82,1216,35,1188,537,4,1042,1338,127,365,198,912,158,1579,714,3791,351,1,840,254,466,1291,3,286,49,5,1046,115,1632,6,313,2076,418,943,520,124,39,191,288,1484,86,12,5,4,1,266,75,6,410,25,759,28,1536,3,645,357,332,553,96,131,4,5,192,220,14,196,95,48,88,6,34,60,15,630,374,291,256,2,156,441,81,7,155,9,519,2,1193,811,1072,3,194,576,230,183,149,241,1303,236,221,312,184,273,637,827,3,2,2,2,21,3,50,71,591,76,97,302,120,218,39,3,1,207,1126,167,236,82,3,399,68,2,562,2,6,1,4,199,23,319,1,2,234,5,6,453,2,345,62,2,121,16,4,112,67,283,161,335,206,38,357,486,163,1,303,1,171,162,828,788,2517,1,12,1,504,269,536,3,775,989,957,2886,21264832,1717,5234,4,2397,5,2992,4,2434,526,3,2846,1072,3,412,15,2325,211,1607,2,1524,210,370,68,297,277,585,377,8497352,138911',kBL:'iaK4',kOPI:89978449};(function(){var a;((a=window.google)==null?0:a.stvsc)?google.kEI=_g.kEI:window.google=_g;}).call(this);})();(function(){google.sn='webhp';google.kHL='en';})();(function(){
var g=this||self;function k(){return window.google&&window.google.kOPI||null};var l,m=[];function n(a){for(var b;a&&(!a.getAttribute||!(b=a.getAttribute("eid")));)a=a.parentNode;return b||l}function p(a){for(var b=null;a&&(!a.getAttribute||!(b=a.getAttribute("leid")));)a=a.parentNode;return b}function q(a){/^http:/i.test(a)&&window.location.protocol==="https:"&&(google.ml&&google.ml(Error("a"),!1,{src:a,glmm:1}),a="");return a}
function r(a,b,d,c,h){var e="";b.search("&ei=")===-1&&(e="&ei="+n(c),b.search("&lei=")===-1&&(c=p(c))&&(e+="&lei="+c));var f=b.search("&cshid=")===-1&&a!=="slh";c="&zx="+Date.now().toString();g._cshid&&f&&(c+="&cshid="+g._cshid);(d=d())&&(c+="&opi="+d);return"/"+(h||"gen_204")+"?atyp=i&ct="+String(a)+"&cad="+(b+e+c)};l=google.kEI;google.getEI=n;google.getLEI=p;google.ml=function(){return null};google.log=function(a,b,d,c,h,e){e=e===void 0?k:e;d||(d=r(a,b,e,c,h));if(d=q(d)){a=new Image;var f=m.length;m[f]=a;a.onerror=a.onload=a.onabort=function(){delete m[f]};a.src=d}};google.logUrl=function(a,b){b=b===void 0?k:b;return r("",a,b)};}).call(this);(function(){google.y={};google.sy=[];var d;(d=google).x||(d.x=function(a,b){if(a)var c=a.id;else{do c=Math.random();while(google.y[c])}google.y[c]=[a,b];return!1});var e;(e=google).sx||(e.sx=function(a){google.sy.push(a)});google.lm=[];var f;(f=google).plm||(f.plm=function(a){google.lm.push.apply(google.lm,a)});google.lq=[];var g;(g=google).load||(g.load=function(a,b,c){google.lq.push([[a],b,c])});var h;(h=google).loadAll||(h.loadAll=function(a,b){google.lq.push([a,b])});google.bx=!1;var k;(k=google).lx||(k.lx=function(){});var l=[],m;(m=google).fce||(m.fce=function(a,b,c,n){l.push([a,b,c,n])});google.qce=l;}).call(this);google.f={};(function(){
document.documentElement.addEventListener("submit",function(b){var a;if(a=b.target){var c=a.getAttribute("data-submitfalse");a=c==="1"||c==="q"&&!a.elements.q.value?!0:!1}else a=!1;a&&(b.preventDefault(),b.stopPropagation())},!0);document.documentElement.addEventListener("click",function(b){var a;a:{for(a=b.target;a&&a!==document.documentElement;a=a.parentElement)if(a.tagName==="A"){a=a.getAttribute("data-nohref")==="1";break a}a=!1}a&&b.preventDefault()},!0);}).call(this);</script><style>#gbar,#guser{font-size:13px;padding-top:1px !important;}#gbar{height:22px}#guser{padding-bottom:7px !important;text-align:right}.gbh,.gbd{border-top:1px solid #c9d7f1;font-size:1px}.gbh{height:0;position:absolute;top:24px;width:100%}@media all{.gb1{height:22px;margin-right:.5em;vertical-align:top}#gbar{float:left}}a.gb1,a.gb4{text-decoration:underline !important}a.gb1,a.gb4{color:#00c !important}.gbi .gb4{color:#dd8e27 !important}.gbf .gb4{color:#900 !important}
</style><style>body,td,a,p,.h{font-family:sans-serif}body{margin:0;overflow-y:scroll}#gog{padding:3px 8px 0}td{line-height:.8em}.gac_m td{line-height:17px}form{margin-bottom:20px}.h{color:#1967d2}em{font-weight:bold;font-style:normal}.lst{height:25px;width:496px}.gsfi,.lst{font:18px sans-serif}.gsfs{font:17px sans-serif}.ds{display:inline-box;display:inline-block;margin:3px 0 4px;margin-left:4px}input{font-family:inherit}body{background:#fff;color:#000}a{color:#681da8;text-decoration:none}a:hover,a:active{text-decoration:underline}.fl a{color:#1967d2}a:visited{color:#681da8}.sblc{padding-top:5px}.sblc a{display:block;margin:2px 0;margin-left:13px;font-size:11px}.lsbb{background:#f8f9fa;border:solid 1px;border-color:#dadce0 #70757a #70757a #dadce0;height:30px}.lsbb{display:block}#WqQANb a{display:inline-block;margin:0 12px}.lsb{background:url(/images/nav_logo229.png) 0 -261px repeat-x;color:#000;border:none;cursor:pointer;height:30px;margin:0;outline:0;font:15px sans-serif;vertical-align:top}.lsb:active{background:#dadce0}.lst:focus{outline:none}</style><script nonce="py6ph9lqL06-MzScU1lnBA">(function(){window.google.erd={jsr:1,bv:2226,de:true,dpf:'b9ABJT_Qle3lZSu3Lz7gnqLjjUwLmbLugWmeHt3LsOA'};
var g=this||self;var k,l=(k=g.mei)!=null?k:1,m,p=(m=g.diel)!=null?m:0,q,r=(q=g.sdo)!=null?q:!0,t=0,u,w=google.erd,x=w.jsr;google.ml=function(a,b,d,n,e){e=e===void 0?2:e;b&&(u=a&&a.message);d===void 0&&(d={});d.cad="ple_"+google.ple+".aple_"+google.aple;if(google.dl)return google.dl(a,e,d,!0),null;b=d;if(x<0){window.console&&console.error(a,b);if(x===-2)throw a;b=!1}else b=!a||!a.message||a.message==="Error loading script"||t>=l&&!n?!1:!0;if(!b)return null;t++;d=d||{};b=encodeURIComponent;var c="/gen_204?atyp=i&ei="+b(google.kEI);google.kEXPI&&(c+="&jexpid="+b(google.kEXPI));c+="&srcpg="+b(google.sn)+"&jsr="+b(w.jsr)+
"&bver="+b(w.bv);w.dpf&&(c+="&dpf="+b(w.dpf));var f=a.lineNumber;f!==void 0&&(c+="&line="+f);var h=a.fileName;h&&(h.indexOf("-extension:/")>0&&(e=3),c+="&script="+b(h),f&&h===window.location.href&&(f=document.documentElement.outerHTML.split("\n")[f],c+="&cad="+b(f?f.substring(0,300):"No script found.")));google.ple&&google.ple===1&&(e=2);c+="&jsel="+e;for(var v in d)c+="&",c+=b(v),c+="=",c+=b(d[v]);c=c+"&emsg="+b(a.name+": "+a.message);c=c+"&jsst="+b(a.stack||"N/A");c.length>=12288&&(c=c.substr(0,12288));a=c;n||google.log(0,"",a);return a};window.onerror=function(a,b,d,n,e){u!==a&&(a=e instanceof Error?e:Error(a),d===void 0||"lineNumber"in a||(a.lineNumber=d),b===void 0||"fileName"in a||(a.fileName=b),google.ml(a,!1,void 0,!1,a.name==="SyntaxError"||a.message.substring(0,11)==="SyntaxError"||a.message.indexOf("Script error")!==-1?3:p));u=null;r&&t>=l&&(window.onerror=null)};})();</script></head><body bgcolor="#fff"><script nonce="py6ph9lqL06-MzScU1lnBA">(function(){var src='/images/nav_logo229.png';var iesg=false;document.body.onload = function(){window.n && window.n();if (document.images){new Image().src=src;}
if (!iesg){document.f&&document.f.q.focus();document.gbqf&&document.gbqf.q.focus();}
}
})();</script><div id="mngb"><div id=gbar><nobr><b class=gb1>Search</b> <a class=gb1 href="https://www.google.com/imghp?hl=en&tab=wi">Images</a> <a class=gb1 href="http://maps.google.com/maps?hl=en&tab=wl">Maps</a> <a class=gb1 href="https://play.google.com/?hl=en&tab=w8">Play</a> <a class=gb1 href="https://www.youtube.com/?tab=w1">YouTube</a> <a class=gb1 href="https://news.google.com/?tab=wn">News</a> <a class=gb1 href="https://mail.google.com/mail/?tab=wm">Gmail</a> <a class=gb1 href="https://drive.google.com/?tab=wo">Drive</a> <a class=gb1 style="text-decoration:none" href="https://www.google.com/intl/en/about/products?tab=wh"><u>More</u> &raquo;</a></nobr></div><div id=guser width=100%><nobr><span id=gbn class=gbi></span><span id=gbf class=gbf></span><span id=gbe></span><a href="http://www.google.com/history/optout?hl=en" class=gb4>Web History</a> | <a  href="/preferences?hl=en" class=gb4>Settings</a> | <a target=_top id=gb_70 href="https://accounts.google.com/ServiceLogin?hl=en&passive=true&continue=http://www.google.com/&ec=GAZAAQ" class=gb4>Sign in</a></nobr></div><div class=gbh style=left:0></div><div class=gbh style=right:0></div></div><center><br clear="all" id="lgpd"><div id="XjhHGf"><img alt="Google" height="92" src="/images/branding/googlelogo/1x/googlelogo_white_background_color_272x92dp.png" style="padding:28px 0 14px" width="272" id="hplogo"><br><br></div><form action="/search" name="f"><table cellpadding="0" cellspacing="0"><tr valign="top"><td width="25%">&nbsp;</td><td align="center" nowrap=""><input name="ie" value="ISO-8859-1" type="hidden"><input value="en" name="hl" type="hidden"><input name="source" type="hidden" value="hp"><input name="biw" type="hidden"><input name="bih" type="hidden"><div class="ds" style="height:32px;margin:4px 0"><input class="lst" style="margin:0;padding:5px 8px 0 6px;vertical-align:top;color:#000" autocomplete="off" value="" title="Google Search" maxlength="2048" name="q" size="57"></div><br style="line-height:0"><span class="ds"><span class="lsbb"><input class="lsb" value="Google Search" name="btnG" type="submit"></span></span><span class="ds"><span class="lsbb"><input class="lsb" id="tsuid_5vkxaLvGJK6g5NoP_smwuAc_1" value="I'm Feeling Lucky" name="btnI" type="submit"><script nonce="py6ph9lqL06-MzScU1lnBA">(function(){var id='tsuid_5vkxaLvGJK6g5NoP_smwuAc_1';document.getElementById(id).onclick = function(){if (this.form.q.value){this.checked = 1;if (this.form.iflsig)this.form.iflsig.disabled = false;}
else top.location='/doodles/';};})();</script><input value="AOw8s4IAAAAAaDIH9r5MMmvFdu6Ikvra9pg-iptnphjH" name="iflsig" type="hidden"></span></span></td><td class="fl sblc" align="left" nowrap="" width="25%"><a href="/advanced_search?hl=en&amp;authuser=0">Advanced search</a></td></tr></table><input id="gbv" name="gbv" type="hidden" value="1"><script nonce="py6ph9lqL06-MzScU1lnBA">(function(){var a,b="1";if(document&&document.getElementById)if(typeof XMLHttpRequest!="undefined")b="2";else if(typeof ActiveXObject!="undefined"){var c,d,e=["MSXML2.XMLHTTP.6.0","MSXML2.XMLHTTP.3.0","MSXML2.XMLHTTP","Microsoft.XMLHTTP"];for(c=0;d=e[c++];)try{new ActiveXObject(d),b="2"}catch(h){}}a=b;if(a=="2"&&location.search.indexOf("&gbv=2")==-1){var f=google.gbvu,g=document.getElementById("gbv");g&&(g.value=a);f&&window.setTimeout(function(){location.href=f},0)};}).call(this);</script></form><div style="font-size:83%;min-height:3.5em"><br></div><span id="footer"><div style="font-size:10pt"><div style="margin:19px auto;text-align:center" id="WqQANb"><a href="/intl/en/ads/">Advertising</a><a href="/services/">Business Solutions</a><a href="/intl/en/about.html">About Google</a></div></div><p style="font-size:8pt;color:#5e5e5e">&copy; 2025 - <a href="/intl/en/policies/privacy/">Privacy</a> - <a href="/intl/en/policies/terms/">Terms</a></p></span></center><script nonce="py6ph9lqL06-MzScU1lnBA">(function(){window.google.cdo={height:757,width:1440};(function(){var a=window.innerWidth,b=window.innerHeight;if(!a||!b){var c=window.document,d=c.compatMode=="CSS1Compat"?c.documentElement:c.body;a=d.clientWidth;b=d.clientHeight}if(a&&b&&(a!=google.cdo.width||b!=google.cdo.height)){var e=google,f=e.log,g="/client_204?&atyp=i&biw="+a+"&bih="+b+"&ei="+google.kEI,h="",k=window.google&&window.google.kOPI||null;k&&(h+="&opi="+k);f.call(e,"","",g+h)};}).call(this);})();(function(){google.xjs={basecomb:'/xjs/_/js/k\x3dxjs.hp.en.8TaJkayy2q0.es5.O/ck\x3dxjs.hp.Bgng2IOHX70.L.X.O/am\x3dAAAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAJSAQRAAAAAIgAEAAAAAAABABACIAAAIAAAAACURAMCBAADQAgCQIABygNIDAAACgAAAAAAAIAAAAAgIAIAAAACAAAAAAAyA8DgmAAAAEAALAAIAAACIRw/d\x3d1/ed\x3d1/dg\x3d0/ujg\x3d1/rs\x3dACT90oFVbZjkzyw5LxZw6kl-5l5VIv7qHw',basecss:'/xjs/_/ss/k\x3dxjs.hp.Bgng2IOHX70.L.X.O/am\x3dAAAAAAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAJSAARAAAAAIAAAAAAAAAAAABACAAAAIAAAAACUBAAAAAADQAgCQIABygNIDAAACgAAAAAAAIAAAAAgIAIAAAACAAAAAAAAAAAgG/rs\x3dACT90oEBUUFVfNKSurOmYSWAnstYDk0Qbw',basejs:'/xjs/_/js/k\x3dxjs.hp.en.8TaJkayy2q0.es5.O/am\x3dAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAAAAgAEAAAAAAABABACIAAAAAAAAAAAQAMCBAACQAgAAAABwAAAAAAAAgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAyA8DgmAAAAEAALAAIAAACIRw/dg\x3d0/rs\x3dACT90oGb7gzO7wkRM6g_Xy1vv17G66t4vg',excm:[]};})();(function(){var u='/xjs/_/js/k\x3dxjs.hp.en.8TaJkayy2q0.es5.O/am\x3dAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAQAAAAAAAgAEAAAAAAABABACIAAAAAAAAAAAQAMCBAACQAgAAAABwAAAAAAAAgAAAAAAAAAAAAAAAAAAAAAAAAAAAAAyA8DgmAAAAEAALAAIAAACIRw/d\x3d1/ed\x3d1/dg\x3d3/rs\x3dACT90oGb7gzO7wkRM6g_Xy1vv17G66t4vg/m\x3dsb_he,d';var st=1;var amd=1000;var mmd=0;var pod=true;var pop=true;var povp=false;var fp='';
var e=this||self;function f(){var b,a,d;if(a=b=(a=window.google)==null?void 0:(d=a.ia)==null?void 0:d.r.B2Jtyd)a=b.m,a=a===1||a===5;return a&&b.cbfd!=null&&b.cbvi!=null?b:void 0};function g(){var b=[u];if(!google.dp){for(var a=0;a<b.length;a++){var d=b[a],c=document.createElement("link");c.as="script";c.href=d;c.rel="preload";document.body.appendChild(c)}google.dp=!0}};google.ps===void 0&&(google.ps=[]);function h(){var b=u,a=function(){};google.lx=google.stvsc?a:function(){k(b);google.lx=a};google.bx||google.lx()}function l(b,a){a&&(b.src=a);fp&&(b.fetchPriority=fp);var d=b.onload;b.onload=function(c){d&&d(c);google.ps=google.ps.filter(function(G){return b!==G})};google.ps.push(b);document.body.appendChild(b)}google.as=l;function k(b){google.timers&&google.timers.load&&google.tick&&google.tick("load","xjsls");var a=document.createElement("script");a.onerror=function(){google.ple=1};a.onload=function(){google.ple=0};google.xjsus=void 0;l(a,b);google.aple=-1;google.dp=!0};function m(b){var a=b.getAttribute("jscontroller");return(a==="UBXHI"||a==="R3fhkb"||a==="TSZEqd")&&b.hasAttribute("data-src")}function n(){for(var b=document.getElementsByTagName("img"),a=0,d=b.length;a<d;a++){var c=b[a];if(c.hasAttribute("data-lzy_")&&Number(c.getAttribute("data-atf"))&1&&!m(c))return!0}return!1}for(var p=document.getElementsByTagName("img"),q=0,r=p.length;q<r;++q){var t=p[q];Number(t.getAttribute("data-atf"))&1&&m(t)&&(t.src=t.getAttribute("data-src"))};var w,x,y,z,A,B,C,D,E,F;function H(){google.xjsu=u;e._F_jsUrl=u;A=function(){h()};w=!1;x=(st===1||st===3)&&!!google.caft&&!n();y=f();z=(st===2||st===3)&&!!y&&!n();B=pod;C=pop;D=povp;E=pop&&document.prerendering||povp&&document.hidden;F=D?"visibilitychange":"prerenderingchange"}function I(){w||x||z||E||(A(),w=!0)}
setTimeout(function(){google&&google.tick&&google.timers&&google.timers.load&&google.tick("load","xjspls");H();if(x||z||E){if(x){var b=function(){x=!1;I()};google.caft(b);window.setTimeout(b,amd)}z&&(b=function(){z=!1;I()},y.cbvi.push(b),window.setTimeout(b,mmd));if(E){var a=function(){(D?document.hidden:document.prerendering)||(E=!1,I(),document.removeEventListener(F,a))};document.addEventListener(F,a,{passive:!0})}if(B||C||D)w||g()}else A()},0);})();window._ = window._ || {};window._DumpException = _._DumpException = function(e){throw e;};window._s = window._s || {};_s._DumpException = _._DumpException;window._qs = window._qs || {};_qs._DumpException = _._DumpException;(function(){var t=[0,16384,0,0,0,0,0,539295744,268439617,100671488,20971520,268435456,8912901,680699392,441450502,544276484,49283081,8537088,1038616352,142606336,132,8388608,8388608,2097154,8454144,805306368,596576256,9,722960,8,292992];window._F_toggles = window._xjs_toggles = t;})();window._F_installCss = window._F_installCss || function(css){};(function(){google.jl={bfl:0,dw:false,eli:false,ine:false,ubm:false,uwp:true,vs:false};})();(function(){var pmc='{\x22d\x22:{},\x22sb_he\x22:{\x22client\x22:\x22heirloom-hp\x22,\x22dh\x22:true,\x22ds\x22:\x22\x22,\x22host\x22:\x22google.com\x22,\x22jsonp\x22:true,\x22msgs\x22:{\x22cibl\x22:\x22Clear Search\x22,\x22dym\x22:\x22Did you mean:\x22,\x22lcky\x22:\x22I\\u0026#39;m Feeling Lucky\x22,\x22lml\x22:\x22Learn more\x22,\x22psrc\x22:\x22This search was removed from your \\u003Ca href\x3d\\\x22/history\\\x22\\u003EWeb History\\u003C/a\\u003E\x22,\x22psrl\x22:\x22Remove\x22,\x22sbit\x22:\x22Search by image\x22,\x22srch\x22:\x22Google Search\x22},\x22ovr\x22:{},\x22pq\x22:\x22\x22,\x22rfs\x22:[],\x22stok\x22:\x222xqGa4L9oN_P6-Eo5hHiqs4roU0\x22}}';google.pmc=JSON.parse(pmc);})();</script></body></html>$ exit
Connection to 127.0.0.1 closed.
ubuntu:/$
```

## Building a Bastion
### Follow the lab here: https://killercoda.com/het-tanis/course/Linux-Labs/210-building-a-bastion-host

```
controlplane:~$ echo "Installing scenario..."
Installing scenario...
controlplane:~$ while [ ! -f /tmp/finished ]; do sleep 1; done
controlplane:~$ echo DONE
DONE
controlplane:~$ ssh node01
Last login: Mon Feb 10 22:06:42 2025 from 10.244.0.131
node01:~$ mkdir /var/chroot
node01:~$ mkdir -p /var/chroot/{bin,lib64,dev,etc,home,usr/bin,lib/x86_64-linux-gnu}
node01:~$ ls -l /var/chroot
total 28
drwxr-xr-x 2 root root 4096 May 24 17:21 bin
drwxr-xr-x 2 root root 4096 May 24 17:21 dev
drwxr-xr-x 2 root root 4096 May 24 17:21 etc
drwxr-xr-x 2 root root 4096 May 24 17:21 home
drwxr-xr-x 3 root root 4096 May 24 17:21 lib
drwxr-xr-x 2 root root 4096 May 24 17:21 lib64
drwxr-xr-x 3 root root 4096 May 24 17:21 usr
node01:~$ cp /usr/bin/bash /var/chroot/bin/bash
node01:~$ for package in $(ldd /bin/bash | awk '{print $(NF -1)}'); do cp $package /var/chroot/$package; done
cp: cannot stat 'linux-vdso.so.1': No such file or directory
node01:~$ for file in passwd group nsswitch.conf hosts; do cp /etc/$file /var/chroot/etc/$file; done
node01:~$ for binary in bash ssh curl; do cp /usr/bin/$binary /var/chroot/usr/bin/$binary; done
node01:~$ ldd /usr/bin/bash
        linux-vdso.so.1 (0x00007ffff8df9000)
        libtinfo.so.6 => /lib/x86_64-linux-gnu/libtinfo.so.6 (0x00007dbd906ba000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007dbd90400000)
        /lib64/ld-linux-x86-64.so.2 (0x00007dbd90863000)
node01:~$ for package in $(ldd /usr/bin/ssh | awk '{print $(NF -1)}'); do cp $package /var/chroot/$package; done
cp: cannot stat 'linux-vdso.so.1': No such file or directory
node01:~$ for package in $(ldd /usr/bin/curl | awk '{print $(NF -1)}'); do cp $package /var/chroot/$package; done
cp: cannot stat 'linux-vdso.so.1': No such file or directory
node01:~$ mknod -m 666 /var/chroot/dev/null c 1 3
node01:~$ mknod -m 666 /var/chroot/dev/tty c 5 0
node01:~$ mknod -m 666 /var/chroot/dev/zero c 1 5
node01:~$ mknod -m 666 /var/chroot/dev/random c 1 8
node01:~$ mknod -m 666 /var/chroot/dev/urandom c 1 9
node01:~$ cp -r /lib/x86_64-linux-gnu/*nss* /var/chroot/lib/x86_64-linux-gnu/
node01:~$ exit
logout
Connection to node01 closed.
controlplane:~$ useradd -m freeuser
controlplane:~$ passwd freeuser
New password: 
Retype new password: 
passwd: password updated successfully
controlplane:~$ ssh node01
Last login: Sat May 24 17:21:11 2025 from 10.244.5.140
node01:~$ useradd -m jaileduser
node01:~$ passwd jaileduser
New password: 
Retype new password: 
Sorry, passwords do not match.
passwd: Authentication token manipulation error
passwd: password unchanged
node01:~$ passwd jaileduser
New password: 
Retype new password: 
passwd: password updated successfully
node01:~$ vi /etc/ssh/sshd_config
node01:~$ systemctl restart ssh
node01:~$ vi /root/bastion.sh
node01:~$ cp /root/bastion.sh /var/chroot/bin/bastion.sh
node01:~$ chmod 755 /var/chroot/bin/bastion.sh
node01:~$ vi /etc/passwd
node01:~$ cp /etc/passwd /var/chroot/etc/passwd
node01:~$ exit
logout
Connection to node01 closed.
controlplane:~$ hostname
controlplane
controlplane:~$ ssh jaileduser@node01
jaileduser@node01's password: 
Make your selection from the items below
You have 20 seconds

1. Connect to controlplane as freeuser.
2. Exit
1
You are being sent to Rocky1
The authenticity of host 'controlplane (172.30.1.2)' can't be established.
ED25519 key fingerprint is SHA256:XikjTSvie4xfSHl8CD14UKeq6b3m5zuJAlVVC6JLlao.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/jaileduser/.ssh' (No such file or directory).
Failed to add the host to the list of known hosts (/home/jaileduser/.ssh/known_hosts).
freeuser@controlplane's password: 
$ id
uid=1001(freeuser) gid=1001(freeuser) groups=1001(freeuser)
$ hostname
controlplane
$ exit
Connection to controlplane closed.
Connection to node01 closed.
controlplane:~$ ssh jaileduser@node01
jaileduser@node01's password: 
Last login: Sat May 24 17:25:24 2025 from 10.244.5.140
Make your selection from the items below
You have 20 seconds

1. Connect to controlplane as freeuser.
2. Exit
You have not entered a valid input
Connection to node01 closed.
controlplane:~$ 

```
### Digging Deeper challenge (not required for finishing lab)
Fix the drawing from the lab with excalidraw and properly replace it here: https://github.com/het-tanis/prolug-labs/tree/main/Linux-Labs/210-building-a-bastion-host

Do a pull request and get some github street cred or something.
 *updated ProLUG lac*
