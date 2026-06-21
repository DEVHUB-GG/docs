# Beat generating

{% stepper %}
{% step %}
### Upload Music

Upload your desired music in `.ogg` format to the `dh_cardance/html/song` folder.
{% endstep %}

{% step %}
### Restart the Server
{% endstep %}

{% step %}
### Join the server

1.  In the **server console**, enter the following command to generate beats for your music file:

    ```bash
    generate_beat file_name.ogg
    ```
2. Do not exit the server until you see the message **"Beat finished successfully"**.
{% endstep %}

{% step %}
### Navigate to the `configs/shared.lua` file

Add your song to `Shared.Sounds` with the following format:

```lua
{ name = "SONG_DISPLAY_NAME", time = "SONG:DURATION", url = "file_name.ogg", diff
```
{% endstep %}

{% step %}
### Restart the Script

Your Car Dancing script should now be fully functional.
{% endstep %}
{% endstepper %}

***

## <mark style="color:yellow;">Example Configuration</mark>

Example of configuration entry for `shared.lua`:

```lua
Shared.Sounds = {
    { name = "Song Display Name", time = "3:45", url = "song_file.ogg", difficulty = "easy" },
    -- Add more songs here
}
```
