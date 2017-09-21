# SecBig4Con
## Android Studio
###LLDB debug native framework
1. command alias bfl breakpoint s -f %1 -l %2
2. p / po / frame select / process attach --pid xxx /process attach --name Safari / list

###reference
1. https://github.com/tiann/android-native-debug
2. https://lldb.llvm.org/tutorial.html


## frida
1. jscode can be written in this way for syntax highlighting : jscode = open('punsec.js').read()
2. in onCreate() get a string of some resource id and get util.Base64 functions' return value:

        Java.perform(function () {

            var MainActivity = Java.use('com.punsec.demo.MainActivity');
            MainActivity.onCreate.implementation = function(a) {
                this.onCreate(a);
                send({'encrypted':this.getString(2131099669)});
            };


            var base64 = Java.use('android.util.Base64');
            base64.decode.overload('java.lang.String', 'int').implementation = function(x, y) {
                // send('Base64 Encoded : ' + x);
                // var buf = new Buffer(base64.decode(x, y));
                // send('Base64 Decoded : ' + buf.toString());
                return base64.decode(x, y);
            }
        });

3. call another func:

            var Util = Java.use('com.punsec.demo.Util');
            Util.a.overload('java.lang.String', 'javax.crypto.SecretKey').implementation = function(x, y) {
                recv('encrypted', function onMessage(payload) { 
                    encrypted = payload['value']; 
                });
                cipher = base64.decode(encrypted, 0); // call the above base64 method
                secret = this.a(cipher, y); // call decrypt method
                send('Decrypted : ' + secret)
                return this.a(secret,y);
            }

4. 
### reference:
1. https://offensivepentest.com/2017/08/26/android-application-reverse-engineering/
2. 

#
