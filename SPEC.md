Note: This specification is not yet finalized.

# DUML specification

This specification describes the DUML (Durable Uniform Minimal Language) file format, parsers of the format, and associated
in-memory objects. It also contains some guidelines for the effective use of DUML by applications.


## DUML nodes

A DUML node is either of:
 
1. an ordered mapping from Unicode strings to DUML nodes (an "object node"); or
2. an ordered list of Unicode strings (a "list node").

In-memory representations of DUML nodes are expected to vary according to the language they support. For example, a DUML
node may be best represented as a tagged union type in a language supporting abstract data types, as a primitive object
or array in a dynamically-typed language, or as multiple classes extending a common supertype in an object-oriented
language. Different parsers in the same language may end up using different choices of representations.

TODO: List operations that 

## DUML file format

A DUML file is a file or Unicode string that represents a DUML object node. Parsers convert a string into an object node
and an ordered list of lost nodes (which are pairs of a list of key strings and a DUML node).

DUML files should have the extension `.duml` and should use UTF-8 encoding if possible.

A DUML file consists of a sequence of lines. Lines are divided by carriage return or newline characters (commonly known
as CR and LF, or as `\r` and `\n`).

(TODO: Issue to resolve before finalizing: It may not be a good idea to allow \0 in keys or values. Should it be
treated as another newline character? Should it be ignored wherever present?)

Empty lines are ignored, with no effect on the root DUML object node. (As a consequence, changing line endings between
`\n`, `\r\n`, and `\r` has no effect on the node.)

Lines beginning with `#` are comments, and are also ignored.

Non-empty, non-comment lines consist of a key and a value. The key consists of all characters before the first space or
tab character. The value consists of all characters after that character. If there is no tab or space character, the
entire line becomes the key and the value is the empty string. (Note that both keys and values may be empty strings.)

Each key-and-value pair then modifies the root object node as follows:

1. The key is divided into a list of key components, delimited by the `.` (period/full stop) character.
2. Track a "current" object node, beginning with the root node, for the current line. For each key component before the
last in the list, check that string's node in the current object node:
  - If it's already an object node, it becomes the new current object node.
  - If there is no node, add an empty object node. It becomes the new current object node.
  - If there is a list node, replace it with an empty object node that becomes the new current object node. Add the
    replaced list node (along with the list of key components for its location) to the end of the list of lost nodes.
3. For the last key component, check that string's node in the current object node:
  - If it's already a list node, use that node in step 4.
  - If there is no node, add an empty list node. Use that node in step 4.
  - If there is an object node, replace it with an empty list node to use in step 4. Add the replaced object node (along
    with the list of key components for its location) to the end of the list of lost nodes.
4. Add the value string to the end of the list of strings in the list node selected in step 3.

After all lines have been processed, the root node and the list of lost nodes are the results of parsing the file.

## Guidelines for use by applications

TODO: Write, explain
