---
description: Documentation for translation.lua Configuration
---

# translation.lua

Every piece of text the script can show, in one file. Around 1,600 entries covering the gym menu, the Gym Creator, the business panel, the admin panel, the minigames, the notifications, the errors and the third-eye target labels.

```lua
Shared.Lang = {
    ['common_save'] = "Save",
    ['gym_dashboard'] = "Dashboard",
    ['notify_exercise_finished'] = "Exercise finished. Reps done: %{reps}",
    ...
}
```

***

## <mark style="color:yellow;">**The three rules**</mark>

{% stepper %}
{% step %}
### Translate the right side, never the left

`['common_save']` is the key the code looks up. `"Save"` is what the player reads. Change the second one only.
{% endstep %}

{% step %}
### Keep every `%{placeholder}` exactly as written

`%{reps}`, `%{name}`, `%{count}` and so on are filled in at runtime. You may **move** one inside the sentence, and you should, because word order changes between languages. You may not rename it, and you may not delete it.

```lua
-- Correct
['notify_exercise_finished'] = "Trening zakonczony. Powtorzenia: %{reps}",

-- Broken, the number will never appear
['notify_exercise_finished'] = "Trening zakonczony. Powtorzenia: %{powtorzenia}",
```
{% endstep %}

{% step %}
### Do not delete keys

A key that is missing or misspelled shows up in game as the key itself, for example `notify_exercise_finished` printed raw in the middle of the interface. If you do not want a text, set it to an empty string rather than removing the line.
{% endstep %}
{% endstepper %}

***

## <mark style="color:yellow;">**How the file is organized**</mark>

The entries are grouped by section, in this order:

| Group | Prefix | What it covers |
| --- | --- | --- |
| Shared labels | `common_` | Save, Cancel, Delete, Search and the rest of the words reused everywhere. |
| Minigames | `minigame_`, and one block per game | The instructions and captions inside each rep minigame. |
| Notifications | `notify_` | The messages that pop up in the corner. |
| Errors | `error_` | Everything a failed action can say back. |
| Target options | `target_` | The third-eye labels: Exercise, Gym Menu, Locker, Cloakroom, Boss Menu. |
| Control hints | `hint_` | The on-screen key hints for the gizmo, prop removal, freecam and exercises. |
| Player gym panel | `gym_` | The whole gym menu. |
| Business panel | `biz_` | The whole business panel. |
| Business admin panel | `admin_` | The whole business admin panel. |
| Gym creator | `creator_` | The whole Gym Creator. |

Finding what you want is easiest by searching for the English text rather than by scrolling to a section.

***

## <mark style="color:yellow;">**Exercise names**</mark>

Every exercise carries a name and a category in its own file under `escrowed/gym/shared/exercises/`. Those are the **fallback**, not the final text. If a matching key exists here, it wins:

```lua
['exercise_bench_press_name'] = "Bench Press",
['exercise_category_strength'] = "Strength",
```

The pattern is `exercise_<id>_name` and `exercise_category_<id>`, where the id is the one in the exercise file. Translating these changes the name everywhere at once: the third-eye option on the machine, the workout catalog in the gym menu, and the exercise picker in the Gym Creator.

{% hint style="info" %}
An exercise you wrote yourself needs no key here. Without one it simply shows the `name` from its own file, so adding exercises never breaks and never forces you to touch this file.
{% endhint %}

***

## <mark style="color:yellow;">**HTML in the values**</mark>

A few entries deliberately contain HTML, and it is safe to keep:

* `<kbd>ENTER</kbd>` renders as a key cap in the on-screen hints.
* `<br>` breaks the line.
* `<b>` and `<strong style='color:#FDD140'>` highlight a word inside a confirmation dialog.

Keep the tags balanced. If you are not comfortable with them, translate only the text between them and leave the tags where they are.

***

## <mark style="color:yellow;">**Special characters**</mark>

The file ships as plain ASCII. Accented and non-Latin characters work, but the file has to be saved as **UTF-8**. Save it in the wrong encoding and the interface shows garbled text.

{% hint style="warning" %}
A `%` sign in a translated value is fine, the script escapes it for you. `"+10% XP"` renders exactly as written.
{% endhint %}

{% hint style="success" %}
Translation changes need a resource restart. Nothing has to be rebuilt: the interface reads the file at startup.
{% endhint %}
