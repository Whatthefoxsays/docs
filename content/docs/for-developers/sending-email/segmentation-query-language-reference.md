---
seo:
  title: Using the SendGrid Segmentation Query Language
  description: Using the SendGrid Segmentation Query Language to segment your contacts
  keywords: API, V3, Segmentation, Manage Contacts
title: Using the SendGrid Segmentation Query Language
group: api-v3
weight: 100
layout: page
navigation:
  show: true
---

**Add introduction here about SGQL**


## Data Types

### Numeric

Any numeric type that can be an integer or float.

- Integer: `[1-9][0-9]*`
- Float: `[0-9]+.[0-9]+`

### String

A set of characters delimited by double or single quotes.

- **Escaping**: Escaping must be done for the character used as the delimiter if it is found within the string. The escape character is \, which must also be escaped with a preceding \.

- **Wildcards**: When using the LIKE or NOT LIKE operators, % will be interpreted as a wildcard character. To escape this character and not treat it as a wildcard, a second % should be used.

### DateTime

A timestamp whose literal value is formatted as a stringsting in the format of ISO 8601: YYYY-MM-DDTHH:mm:SSZ(-)HH:mm  

### Interval

A time interval with an integral scalar value and some unit of time which can be one of the following: second, minute, hour, day, month, or year.

### Boolean

Boolean values are true or false.

### Null

Null is a special type that represents a lack of a value.

## Operators

### Logical

<table>
  <tr>
    <th>Operator</th>
    <th>Associativity</th>
    <th>Operands</th>
  </tr>
  <tr>
    <td>And</td>
    <td>Left</td>
    <td>2</td>
  </tr>
  <tr>
    <td>Or</td>
    <td>Left </td>
    <td>2</td>
  </tr>
  <tr>
    <td>Not</td>
    <td>Right</td>
    <td>2 (binary)</td>
  </tr>
  <tr>
    <td>Not</td>
    <td>Right</td>
    <td>1 (unary)</td>
  </tr>
</table>


### Arithmetic

Precedence from low to high:

   <table>
  <tr>
    <th>Operator</th>
    <th>Associativity</th>
    <th>Operands</th>
    <th>Supported Types</th>
  </tr>
  <tr>
    <td>-</td>
    <td>Left</td>
    <td>2 (binary)</td>
    <td>Numeric - Numeric<br>DateTime - Interval</td>
  </tr>
  <tr>
    <td>+ </td>
    <td>Left</td>
    <td>2</td>
    <td>Numeric + Numeric<br>DateTime + Interval<br>String + String (concatenation)</td>
  </tr>
  <tr>
    <td>/</td>
    <td>Left</td>
    <td>2 </td>
    <td>Numeric / Numeric</td>
  </tr>
  <tr>
    <td>*</td>
    <td>Left</td>
    <td>2</td>
    <td>Numeric * Numeric</td>
  </tr>
  <tr>
    <td>%</td>
    <td>Left</td>
    <td>2</td>
    <td>Numeric % Numeric (modulo)</td>
  </tr>
  <tr>
    <td>- </td>
    <td>Left</td>
    <td>1 (unary)</td>
    <td>- Numeric</td>
  </tr>
</table>

### Comparison

<table>
  <tr>
    <th>Operator</th>
    <th>Supported Types (T represents any type)</th>
  </tr>
  <tr>
    <td>=</td>
    <td>T = T</td>
  </tr>
  <tr>
    <td>!=</td>
    <td>T != T</td>
  </tr>
  <tr>
    <td>&lt;</td>
    <td>Numeric &lt; Numeric<br>DateTime &lt; DateTime<br>String &lt; String</td>
  </tr>
  <tr>
    <td>&gt;</td>
    <td>Numeric &lt; Numeric<br>DateTime &lt; DateTime<br>String &lt; String</td>
  </tr>
  <tr>
    <td>&lt;=</td>
    <td>Numeric &lt; Numeric<br>DateTime &lt; DateTime<br>String &lt; String</td>
  </tr>
  <tr>
    <td>&gt;=</td>
    <td>Numeric &lt; Numeric<br>DateTime &lt; DateTime<br>String &lt; String</td>
  </tr>
  <tr>
    <td>LIKE/ NOT LIKE</td>
    <td>String (NOT) LIKE String</td>
  </tr>
  <tr>
    <td>IS (NOT)</td>
    <td>T is (NOT) NULL</td>
  </tr>
  <tr>
    <td>(NOT) IN</td>
    <td>T IN (T)</td>
  </tr>
  <tr>
    <td>(NOT) BETWEEN</td>
    <td>Numeric (NOT) BETWEEN Numeric AND Numeric<br>DateTime (NOT) BETWEEN DateTime AND DateTime<br>String (NOT) BETWEEN String AND String</td>
  </tr>
</table>

## Identifiers

Identifiers are named things within a given query. These include both function names and field/column names. Identifiers cannot be a keyword and must only allow the characters: [a-zA-Z_]+. 

<call-out>

Identifiers that do not meet the previous format may still be used.  However, they must be encapsulated within backticks.  I.E `000supercoolid`

</call-out>

## Functions

Functions can be invoked with or without parameters by providing the function name (identifier) followed by a list of arguments separated by commas enclosed in a set of parentheses.

`MY_FUNCTION(a,b,c)`

### Well Defined Functions

These are functions that should be used consistently across consumers of the parser. Whether or not your implementation actually supports them is up to you.

**CONTAINS(array_or_map, value_or_key)**

Contains should return a boolean indicating the presence of a value in an array or map. When used with an array, true should be returned when the array holds the given value. When used with a map, true should be returned when the map has an element with the given key.

**CONCAT(s1,s2)**

Concat takes two strings and combines them as a single string in that order and returns the result.

**LENGTH(s)**

Length takes a single string and returns the number of characters in the string.

**LOWER(s)**

Lower returns a lowercased version of the given string.

**NOW()**

Returns the current date and time.

## Fields

A number of fields are available on every contact. These include the strings:
- `primary_'email'`
- `primary_'email_domain'`
- `first_name`
- `last_name`
- `address_line_1`
- `address_line_2`
- `city`
- `state_province_region`
- `country`
- `postal_code` 

<call-out>

In the future, the address fields may be used with a third-party service to populate a `location` type field when contacts are added or updated.) In addition, contacts have `alternate_emails` and `alternate_email_domains` fields that represent sets of strings.

</call-out>

Note that the `list_id` attribute is no longer present in the top-level object. It is instead replaced by a `list_ids` field that is a set of IDs. Making `list_ids` a regular contact field allows, for example, specifying multiple lists from which to draw contacts, or that each contact must be a member of multiple lists. Similarly, there is a `segment_ids` field that allows creating conditions specifying membership in other segments. (Conditions on `segment_ids` will need to be checked to ensure they don’t create circular dependencies or extreme levels of nesting.)

Besides `list_ids` and `segment_ids`, the fields created by event queries are also set fields, also known as unordered collections. Collections are fields containing multiple values. They may be ordered, in which case they are called arrays. Array elements may be accessed by their index using the `[]` operator. For example, if `products` is an array of strings, `products[0] IS 'pet rock'` would mean that the first element of `products` must be 'pet rock'. This operator can also be used with a variable inside the brackets, for either type of collection, indicating that any element in the collection may be used to satisfy the condition. If multiple conditions specify the same variable on the same field, then the same element in each collection must be used to satisfy each of those conditions. If a variable is only used in one condition, it may be omitted (e.g. `products[]`).

Variables used with the `[]` operator are particularly useful for specifying multiple conditions on a map contained in a collection. Maps are essentially collections of key-value pairs, where the keys are strings. For both collections and maps, the values may either be of the five primitive data types, or collections or maps themselves. The values in maps are referenced by specifying the desired keys inside the `[]` operator as quoted strings.

To see how collections and maps might be used together with variables, consider the following segment (where products is now an array of maps):

```
{
	"name": "Suckers",
"query_dsl": "
		WITH junk_order AS EVENTS(
			type IS 'order' 
AND products[x]['name'] IS 'junk' 
AND products[x]['price'] > 500"
)

COUNT(junk_order) > 0
"
}
```
Note that in the final two conditions in the `junk_order` query, the variable `x` is used as the index for the `products` collection, indicating that the product with the name "junk" must be the same as the one that costs over $500 (or 500 of whatever currency unit `price` represents).

We just saw how variables are used to indicate that any element of a collection may satisfy a condition. To indicate that every element in a collection must satisfy a condition, negate both its comparison operator and the condition itself. Consider the following segment:

```
{
	"name": "Lovers",
	"query_dsl": "NOT CONTAINS(list_ids, <Fighters ID>)"
}
```

This segment represents lovers as anyone not on the "Fighters" list.
Use cases/ code samples

https://github.com/sendgrid/mc-sgql-snowflake/blob/master/lib/translation/field/reserved_fields.go#L15
