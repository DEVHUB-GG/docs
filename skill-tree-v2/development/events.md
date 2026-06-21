# Events

## <mark style="color:yellow;">Server Events</mark>

### Reset Skill Category

Reset player skill tree category, clear all unlocked skills and return points,\
<mark style="color:$warning;">**The event will skip all requirements for reset.**</mark>

#### Skill Reset Event will be triggered

{% content-ref url="listeners.md" %}
[listeners.md](listeners.md)
{% endcontent-ref %}

<pre class="language-lua"><code class="lang-lua"><strong>TriggerServerEvent('devhub_skillTree:server:resetSkillCategory', categoryUid)
</strong>--- EXAMPLE
TriggerServerEvent('devhub_skillTree:server:resetSkillCategory', 'personal')
</code></pre>

**Parameters:**

* `categoryUid` (string): Category name (e.g., 'personal')

***

### Clear Skill Category

Reset player skill tree category, clear all unlocked skills , xp and level, <mark style="color:$danger;">**POINTS WILL NOT BE RETURNED**</mark>.

#### Skill Reset Event will be triggered

{% content-ref url="listeners.md" %}
[listeners.md](listeners.md)
{% endcontent-ref %}

<pre class="language-lua"><code class="lang-lua"><strong>TriggerServerEvent('devhub_skillTree:server:clearSkillCategory', categoryUid)
</strong>--- EXAMPLE
TriggerServerEvent('devhub_skillTree:server:clearSkillCategory', 'personal')
</code></pre>

**Parameters:**

* `categoryUid` (string): Category name (e.g., 'personal')
