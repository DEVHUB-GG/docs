# ❓ How to create table



{% stepper %}
{% step %}
## Change the config

* Go to the `configs/client.lua` folder, find `Config.DeveloperMode`, and set it to `true`
{% endstep %}

{% step %}
## Restarting the Script

* Restart the script, then in the game, use the command `tablegenerator` to open the table creator.
{% endstep %}

{% step %}
## **Table settings**

* Select the table UID; this will be useful later for triggering the event to open the correct table.
* Choose the table prop (it will automatically update after pressing confirm).
* If you have our script on SkillTree, you can enable XP gain after a successful craft.
{% endstep %}

{% step %}
## **Rewards**

* You can specify which items the player should have after a successful craft.
{% endstep %}

{% step %}
## Needed **items**

* You can specify which items the player must have in their inventory to start crafting.
{% endstep %}

{% step %}
## Props

### **1. Setting the Prop’s Initial Position**

Define the starting position where the prop will first appear in the game world.

### **2. Setting the Prop’s Final Position**

Specify the destination to which the player must move the prop. This marks the end location the prop should be transferred to.

### **3. Configuring Particles (Optional)**

You can enhance the prop with particle effects and sound effects.

* **Particle Settings (Optional):**
  * Set the particle library and particle name.
  * Adjust the particle scale (Optional).
  * If no particle effect is needed but other options are required, leave these fields empty.
* **Sound Settings (Optional):**
  * Provide a URL for the sound effect.
  * If no sound effect is needed, leave this field empty.
  * (The framework used for sound can be modified here.)
* **Additional Particle Properties:**
  * Define the duration for which the particle effect should last.
  * Choose whether the prop should be deleted after the animation finishes (1 = Yes, 0 = No).

***

## **4. Additional Prop Features**

Each prop can be customized with different functionalities:

| Feature          | Description                                                                                              |
| ---------------- | -------------------------------------------------------------------------------------------------------- |
| **Static Prop**  | The prop remains in a fixed position.                                                                    |
| **Spawn Delay**  | The prop appears **with a delay**, making it visible only when its turn comes.                           |
| **Model Change** | The prop **changes its model or location** at a specific step (e.g., a pot changes when water is added). |

These settings allow you to create a more dynamic and interactive experience using our table.


{% endstep %}

{% step %}
## Saving & Applying the Configuration

Once all settings are configured:

1. **Press the "Save" button** in the Table Generator.
2. **Open** the `configs/shared.lua` file.
3.  **Find** the following configuration:

    ```lua
    Shared.Recepie = {
        -- Generated code
    }
    ```
4. **Paste the generated table code inside `Shared.Recepie`**.
{% endstep %}
{% endstepper %}
