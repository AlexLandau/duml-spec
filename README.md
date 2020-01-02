# duml-spec

(This is in-progress.)

This repository contains the specification for the DUML (Durable Uniform Minimal Language) file format. DUML is an
alternative to formats like XML, JSON, YAML, and TOML for configuration files: files that are checked into source
control and control the behavior of an application or build tools.

Along with [the specification](SPEC.md), more informal information is available below.

# Examples

These examples show DUML files along with a JSON representation of equivalent objects.

```
# Simple key-value pairs, separated by the first space or tab in the line
name duml-spec
owner AlexLandau
description The specification for the Durable Uniform Minimal Language file format.

# A key by itself is given an empty string as a value
public

# Keys can have multiple values
related AlexLandau/duml-java
related AlexLandau/duml-js

# Dots in keys are used for namespacing
collaborators.owners AlexLandau
```

``` json
{
    "name": ["duml-spec"],
    "owner": ["AlexLandau"],
    "description": ["The specification for the Durable Uniform Minimal Language file format."],
    "public": [""],
    "related": ["AlexLandau/duml-java", "AlexLandau/duml-js"],
    "collaborators": {
        "owners": ["AlexLandau"]
    }
}
```

# Comparison

Compare the following properties:

- Single unambiguous specification
- Supports comments
- Unaffected by \r\n vs. \n
- Amenable to regular expression-based tooling
- Errors can't cause the parser to refuse to give anything to the application
- Maybe same as above: Partially-invalid "durability" (messing up one part of the file has limited impacts on whether other parts of the file can be read)
- Can paste in blocks of config anywhere and they're likely to work
- Unusually easy to parse/write a parser
- Supports "namespacing"
And the drawbacks:
- Values cannot directly contain newlines
- Keys cannot contain periods
- Cannot have lists/arrays of objects
- Highly nested objects may be more verbose (but at least they compress well?)
Then some things are mixed good/bad, like lack of canonical validation scheme (like xsd) or data types, or "whitespace sensitivity", or lack of escaping.

Subjectively, it's a very regular language and it's easy to learn all the edge cases. (TODO: Link to a tutorial/introduction if haven't already)

Maybe also bring up end-to-end principle

# Implementations

Java/JVM: duml-java (TODO: link)
Javascript: TBD
