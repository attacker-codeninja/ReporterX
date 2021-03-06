# A introduction to yaml templates for RepoterX
***This page contains the information about how these templates can be used and how can you make [own templates](#How-to-make-own-templates).***
**Templates can be used according to vulnerability.**

Vulnerability | How to
--- | ---
[Broken link hijacking](https://github.com/RESETHACKER-COMMUNITY/ReporterX/tree/main/templates/Broken-link-hijacking) | Host will be used as the page where the broken link is found.<br> Parameter will be used as the broken link that is found.
[XSS - Relected](https://github.com/RESETHACKER-COMMUNITY/ReporterX/tree/main/templates/Cross-site-scripting/Reflected) | Host will be the url with path of the vulnerable page.<br> Parameter will be the vulnerable parameter with payload.
[No rate-limiting](https://github.com/RESETHACKER-COMMUNITY/ReporterX/tree/main/templates/No-rate-limiting) | Host will be the url with path of the vulnerable page.<br> Parameter can be used as the placeholder for the parameter<br> to select in intruder.
[Open redirects](https://github.com/RESETHACKER-COMMUNITY/ReporterX/tree/main/templates/Open-redirects) | Host will be the url and parameter will be the vulnerable parameter<br> with payload.
[SQLI - GET Based](https://github.com/RESETHACKER-COMMUNITY/ReporterX/tree/main/templates/SQL-inection) | Host will be the url and parameter will be the vulnerable parameter<br> with payload.
[SSRF - GET Based](https://github.com/RESETHACKER-COMMUNITY/ReporterX/tree/main/templates/Server-side-request-forgery) | Host will be the url and parameter will be the vulnerable parameter<br>with payload.

<p>If you create any new vulnerability's template update it in this table.</p>

-----

# How to use special characters in arguments
**To use special characters, There are several ways of it. Here are some :**

Character | Name | Windows | Linux
--- | --- | --- | --- |
' | Single quote | You can directly use single quotes in windows powershell or<br>You can use them inside two double quotes | Use backslash(\) before a single quote.
" | Double quote | Use a backslash before using a double quote | Use inside two single quotes with a backslash(e.g. -p ?id=1'\"').
< | Less than | Use inside two double quotes(e.g. -p "<") | Use a backslash before less than.
\> | Greater than | Use inside two double quotes(e.g. -p "<") | Use a backslash before Greater than.
/ | Forward slash | Directly usable | Directly usable.
\` | Carrot | Directly usable | Use a backslash before carrot.
$ | Dollar | Directly usable | Use backslash before dollar.
% | Percentage | Direclty usable | Direclty usable.


# How to make own templates
**Here is how can you create own templates :**

The templates are made in yaml, to read more yaml you can read [this](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html).

***Let's create a new template for sql injection.*** 

**The templates should starts with a unique id, You can use like this**
```
# id is a unique identifier for our template. id must not contain spaces.
id: SQLI-GET-1
```

### Information about our report.
**This dictionary contains the details about the report we are going to write.**
```
info:
  name: SQL Injection on GET parameter. # Name of the vulnerability
  author: coder_rc # Author's name/twitter username.
  severity: Critical # Severity of the vulnerability.
  category: SQL Injection # Category of vulnerability, maybe according to hackerone or bugcrowd.
  language: en # Language
  required: # This is a list which contains the required arguments for your template.
    - url # User can specify this by using -u, --url. You can access this inside your reports by using {{host}}. This will contain the domain + path of the vulnerable page.
    - param # User can specify this by using -u, --url. This can be the vulnerable parameter with the value or in the case of Broken link hijacking or same of kind of vulns, it can be the broken link. You can access this by using {{param}}.
    - reporter_username # User can specify this by using -user, --user. You can use this at the end of the report, For example : Many people like to write Best regards, @username at the end of their report, you can use it for that purpose. Can be accessed by {{username}}.
    - steps # This is used to tell the tool that your template actually needs steps to reproduce, In some cases the vulnerability doesn't requires steps to reproduce. Users can specify a filepath by using -s, --steps that will be replaced by our actual steps to reproduce that we wrote.
    - impact # Same as steps to reproduce, To tell the tool that your report actually have a impact and can be overwrited by user with -imp and --impact switch.
    - remediation # Same as impact, To tell the tool that your report actually have a remediation and can be overwrited by user with -rem and --remediation switch. 
```

## Writing the actual report.
**This dictionary contains our actual report.**
```
report:
  summary: "
  ## Summary
\nHello there,
\nI hope you are doing great!,
\nI have found a SQL Injection vulnerability on one of the [domain]({{host}}) mentioned in the scope.\n
\nVulnerable host: {{host}}
\nVulnerable parameter: {{param}}
"
  #This summary blocks contains the summary of our report. Here you can see how we can use the host and param variables. Every line in part of reports should start with a \n, So that every line looks different. 
  
  impact: "## Impact
\nAn attacker can use SQL injection it to bypass a web application’s authentication and authorization mechanisms and retrieve the contents of an entire database.
\nSQLi can also be used to add, modify and delete records in a database, affecting data integrity.
\nUnder the right circumstances, SQLi can also be used by an attacker to execute OS commands, which may then be used to escalate an attack even further."
  
  
  remediation: "## Mitigation
\nSanitize all the user inputs."

  end: "Best regards,
\n{{username}}
"
```

## A quick summary what we have learn.
| Name | How is it used |
| ---- | -------------- |
| id   | Unique id of your template |
| info block | The info about the report block|
| report block | Our actual report |

**If you feel you can improve this documentation, Create a pull request now!**

***Wrote with <3 by [RC](https://twitter.com/coder_rc)***
