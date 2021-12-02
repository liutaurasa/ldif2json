# ldif2json
AWK script to convert LDIF to JSON

This is a simple script which converts LDIF to JSON. My primary use case is to import read generated JSON with Ansible.
The script accepts variables supplied after as values for -v (awk standart way to supply variables)
format={dict|list} - what kind of JSON should be produced (see below). dict - will be a dictionary, list - list of dictionaries
key=<attribute> - if format is dict, then you can choose what LDIF attribute should be used as basis for the dict.
records=<n> - how many records to read. Good for testing.


To generate JSON list of dictionaries use:
```
ldapsearch <your ldap search parameters> | /usr/local/bin/ldif2json -v format=list
```
the output will be something like:
```
[
  {
    "dn": "replaced dn",
    "cn": "replaced cn",
    "departmentNumber": "replaced departmentNumber",
    "displayName": "replaced displayName",
    "givenName": "replaced givenName",
    "l": "replaced l",
    "mail": "replaced mail",
    "o": "��eiǝ",
    "sn": "replaced sn",
    "title": "replaced title",
    "objectClass": [
      "replaced objectClass",
      "replaced objectClass",
      "replaced objectClass",
      "replaced objectClass",
      "replaced objectClass"
    ]
  },
  {
  ...
  }
]
```

Command `ldapsearch <your search parameters> | | /usr/local/bin/ldif2json -v format=dict | jq` will produce a dictionary with every key in the dictionary being LDAP object DN and that key values will be all attributes of that object. You can add change key with command: `ldapsearch <your search parameters> | | /usr/local/bin/ldif2json -v format=dict key=nsuniqueid | jq` to change JSON dict key value to nsUniqueId attribute value.

```
{
  "replaced dn": {
    "dn": "replaced dn",
    "cn": "replaced cn",
    "departmentNumber": "replaced departmentNumber",
    "displayName": "replaced displayName",
    "givenName": "replaced givenName",
    "l": "replaced l",
    "mail": "replaced mail",
    "o": "��eiǝ",
    "sn": "replaced sn",
    "title": "replaced title",
    "objectClass": [
      "replaced objectClass",
      "replaced objectClass",
      "replaced objectClass",
      "replaced objectClass",
      "replaced objectClass"
    ]
  },
  "....": {}
}
```
