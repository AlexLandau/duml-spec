# Design principles of DUML

Configuration files: DUML is designed for the specific purpose of configuration files that are read by applications to
specify or modify their behavior. This includes files for configuring build tooling that are checked into source control
with a codebase. These files may be written by humans, tools, or both, and will be read by both. This means that support
for comments is necessary.

Determinism: All DUML parsers should produce the same results for the same file.

Simplicity and uniformity: The DUML format is simple enough to be learned and understood quickly.

Durability: Errors in one part of a file don't prevent other parts of the file from being read and used by the
application. For most config file formats involving parsing, a syntax error means the application has no way to get any
information from the file; DUML defines these errors out of existence so this situation is avoided.

Easy file manipulation with general-purpose tools: DUML configurations can be modified by appending new lines to the end
of the file, and can be searched or modified with regular-expression-based tooling. Many config file formats use nested
structures that cannot be reliably handled by regular expressions, and which make copying and pasting blocks of
configuration more difficult.

Application-level error handling: Error messages from a file parser are often less useful than the error messages the
application can provide. This goes both for missing keys and for inappropriate value types. DUML passes its information
about the file to the application to allow it to handle errors and warnings in whatever manner is most appropriate. 

## Tradeoffs made

Limits (or lack thereof) on keys and values: The format imposes some restrictions on its keys and values that may be
undesirable for some use cases. In particular, values cannot contain newlines, requiring workarounds that may vary by
application. Keys cannot contain `.`, which could be useful for some applications, such as assigning values to web
addresses. Conversely, the broad range of possible keys could be an issue for implementations in some programming
languages.

Application-dependent parsing of specialized types: Some configuration formats include parsing of specific data types,
such as TOML's parsing of dates. There are some advantages to DUML's approach of not parsing these directly and allowing
the application to parse them, such as not imposing whether numbers are integers or floating-point or their maximum
values. However, an advantage of parsing more complicated values (like dates) within the config format is that it's
often easier to find answers to questions about the format of dates within the configuration language as opposed to an
individual application's configuration (which could vary depending on the exact libraries used by the application).

Repeated keys: The repetition of parts of keys can lead to larger file sizes compared to other formats. This is somewhat
mitigated by the high compressibility of this repetition, and the fact that configuration files are rarely large.
