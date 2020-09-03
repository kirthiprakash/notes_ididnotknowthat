# Android Firewall - Block internet for select apps

Every Android application out there wants to have full internet access. Applications like calculator, SMS messager and Phone call apps doesn't really need internet to provide core functionality. It would be great if users were allowed to block internet access to such apps despite providing it with the required permissions.

MIUI had a nice in-build firewall application which allows users to allow/disallow internet connectivity for an individual app.

Stock android doesn't have such in-build firewalls. There are tons of firewall apps on Google Play store but  most of them require full internet access themselves.

There are a few firewall apps like **Karma FW** app which requires **ZERO** permissions, allows users to specifically select a bunch of apps for which it should block internet. Its internally uses Android VPN to monitor all the network traffic going in and out of the phone.

{% embed url="https://play.google.com/store/apps/details?id=eu.stargw.fok" %}

What would be more awesome is, if the firewall can block only outbound traffic and allow incoming traffic for a specific application, just like we configure firewalls on our computers, that would make sure our data is safe and not being uploaded to some servers in china. 



