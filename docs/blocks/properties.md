# Block properties

Block properties are the configuration fields of a block.

<img class="right shadow" src="/img/blocks/modifier-block-config.png" width="225" />
A **block type name**—_Filter_, _Modifier_, _CounterIntervalSimulator_—defines the block type and the configurable fields. (In the System Designer, this name is abbreviated in a vertical bar on the left side of the block.)

When you drag a new instance of a block type onto the canvas of the System Designer, you will see that block's available configuration fields.

When you save your block, its configuration is saved and you can find that configured block under the **configured** tab of the block library. Each instance of a configured block will have a unique block ID that is visible under the name field.

As an example, the configuration panel for a _Modifier_ block is shown at the right. It displays four properties of the _Modifier_ block:
- **Exclude existing fields**
- **Fields**
- **Log Level**
- **Version**

---

## Property types

In the example above, **Exclude existing fields** is a checkbox, **Fields** is a list of input pairs, **Log Level** is a drop-down menu, and **Version** is a series of three numbers separated by periods (0.1.0).

The different types of inputs in the configuration panel represent different property types. Below you’ll find a list of the property types used in nio Blocks.

- **BoolProperty**<br>Represented by a checkbox, and can equal either True or False.
- **FileProperty**<br>Reference to a local file to be opened and read.
- **FloatProperty**<br>Can only be a float.
- **PropertyHolder**<br>Provides base functionality of all properties.
- **IntProperty**<br>Can only be an integer.
- **ListProperty**<br>Represented by a list of items defined by the user with **+ Fields** to add items and **x** to remove items.
- **ObjectProperty**<br>Can only be a dictionary.
- **SelectProperty**<br>Represented by a drop-down menu, and enumerates the available options included in the drop down menu.
- **StringProperty**<br>Can only be a string.
- **TimeDeltaProperty**<br>Requires integer values in minutes, seconds, and milliseconds.
- **VersionProperty**<br>Specifies the version of the block type. It represents the block’s version when this configuration was created. One should only update the version property of a block configuration once they have updated a block’s version and have confirmed that the block configuration works with the new version of the block.

Some properties can take any type of input while others require specific types of input. The IntProperty can only take integers while the SelectProperty requires a selection from a specific list. If your property input is filled with the incorrect type of content, your block will not configure and your service will not start.

For more information on applying these properties, please feel free to check out our GitHub repository for the nio framework <https://github.com/niolabs/nio/tree/master/nio/properties>.

In the above example
- **Exclude existing fields** is a "BoolProperty" that can be true or false and is represented by a checkbox.
- **Log Level** is a "SelectProperty" that enumerates available options ("Not set", "Debug", "Info", "Warning", "Error") and is represented by a drop-down menu.
- **Fields** is a "ListProperty" and is represented by a list of items that can be defined by the user with **+ Fields** to add items and **x** to remove items. In this example, there are three key-value pairs defined in the list. Each item in a list can also have a type, in this case, a dictionary defined by a "PropertyHolder" property made up of two properties represented by the **Attribute Name** and **Attribute Value** input areas.
- **Version** is a special "VersionProperty" that takes three integers separated by periods.

This _Modifier_ block is named **ShowAlertLevel** and is configured to enrich the signal with three new fields, one each for danger, caution, and success. The structure of the fields added to the signal would look similar to the following. (For illustration purposes, the expressions in curly braces have not been evaluated.)

```python
[
  {
    "attribute_name": "danger_label",
    "attribute_value": {{ $alert_value }}
  },
  {
    "attribute_name": "caution_label",
    "attribute_value": {{ $warning_value }}
  },
  {
    "attribute_name": "success_label",
    "attribute_value": {{ $ok_value }}
  },
]
```

---

## Common mixins

Some commonly used block properties are [mixins](/blocks/block-development/mixins.md) and include `enrich`, which creates the **Exclude Existing?** property, `persistence`, which creates the **Load from Persistence?** property, and `group-by` which creates the **Group By** input field. For an example of how to configure and use `group-by` using the _Join_ block, see [restructure and group signals](/service-design-patterns/join.md#using-group-by) in [service design patterns](/service-design-patterns/README.md).

---

## Expressions

Expressions are used to dynamically define properties. Many property types can accept expressions. Expressions are found inside the double curly braces. In the _Modifier_ block configuration shown above, you can see example expressions in the **Attribute Value** inputs. Learn more about how to create expressions in [nio Expressions](/blocks/expressions.md).

---

## Environment variables

Another syntax that the nio Platform uses in property configuration is the double-square-bracket notation for an [environment variable](/instances/environment-variables.md).

`[[YOUR_ENV_VAR]]`

Environment variables are used when you want your instance to function in different environments or you do not want to expose an access token or other secret.

Your environment variable can be used alone, or you can embed it, with the double-square-bracket notation, inside of strings and expressions.

To embed your environment variable inside of single brackets, you need to include a space between the single bracket and the double brackets of the environment variable

```[ [[MY_ACCESS_TOKEN]] ]```
