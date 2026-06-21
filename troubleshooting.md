---
description: General Troubleshooting Checklist
---

# ⚠️ Troubleshooting

### <mark style="color:yellow;">**Before reporting issues, verify:**</mark>

* [ ] The resource is installed correctly and started
* [ ] devhub\_lib [devhub\_lib-needed-for-each-script](scripts/devhub_lib-needed-for-each-script/ "mention")is ensured after your core but as first devhub script
* [ ] Your framework is working correctly and is configured in [devhub\_lib-needed-for-each-script](scripts/devhub_lib-needed-for-each-script/ "mention")
* [ ] Run compatibility test in devhub\_lib to check your framework configuration [compatibility-test.md](scripts/devhub_lib/compatibility-test.md "mention")
* [ ] Fix all errors or warnings from [compatibility-test.md](scripts/devhub_lib/compatibility-test.md "mention")
* [ ] All dependencies are installed. (look into **Installation** section of each individual script)
* [ ] Database tables are created from sql file (if provided)
* [ ] Config files have no syntax errors [#verify-config-syntax](troubleshooting.md#verify-config-syntax "mention")
* [ ] Server and resource are fully restarted
* [ ] Debug mode is enabled to see error messages
* [ ] Console (F8 and server) checked for errors
* [ ] No conflicting resources are running
* [ ] Game cache is cleared (if using custom assets)

### <mark style="color:yellow;">**If issues persist:**</mark>

1. Make fresh script resintall
2. Enable debug mode for detailed logs
3. Check client (F8) , server and NUI console
4. Test with minimal configuration (do not change anything in the config)
5. Verify issue on clean server with only required resources
6. Check for updates to tested script and devhub\_lib&#x20;
7. Contact DevHub support with:
   * Server console logs
   * Client console logs (F8)
   * NUI console logs (F8 NUI devtools)
   * Config files
   * Steps to reproduce the issue
   * Results from [compatibility-test.md](scripts/devhub_lib/compatibility-test.md "mention")



### <mark style="color:yellow;">Verify Config Syntax</mark>

* Check all config files for syntax errors:
  * Missing commas in tables
  * Mismatched brackets `{}`, `[]`
  * Unclosed strings `""`
  * Invalid Lua syntax
* Use a Lua syntax validator or IDE with Lua support to check files.
