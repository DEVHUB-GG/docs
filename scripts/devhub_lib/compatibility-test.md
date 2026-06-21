# 🆘 Compatibility test

To ensure the server configuration is working correctly, run a compatibility test

{% stepper %}
{% step %}
### Go to <kbd>config.lua</kbd> and set **Shared.CompatibilityTest** to true
{% endstep %}

{% step %}
### Add new item named dh\_test
{% endstep %}

{% step %}
### Restart your server
{% endstep %}

{% step %}
### Join the server and type /dh\_startTest to start the test
{% endstep %}

{% step %}
### Follow the instructions shown in the game.
{% endstep %}

{% step %}
### Check the server console for the test results
{% endstep %}

{% step %}
### In the devhub\_lib files, you will find file with test results. Please send them if requested by devhub support.


{% endstep %}
{% endstepper %}

{% hint style="info" %}
During the test, you will need to follow on-screen guides and perform specific actions, such as using an item and reviving yourself. Afterward, you will be kicked from the server. Results will be displayed in the server console within 10 seconds of being kicked.
{% endhint %}

<figure><img src="https://upload.devhub.gg/dh_upload/docs/scripts-1.webp" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
At the end of a test, if any scenario fails, you will see tips and a to-do list with the exact file path you need to open to adjust the functionality to work with your server requirements.
{% endhint %}

<figure><img src="https://upload.devhub.gg/dh_upload/docs/scripts-2.webp" alt=""><figcaption></figcaption></figure>
