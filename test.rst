Capabilities Reference
======================

Capabilities are core to the SmartThings architecture. They allow us to abstract specific devices into their underlying capabilities.

An application interacts with devices based on their capabilities, so once we understand the capabilities that are needed by a SmartApp, and the capabilities that are provided by a device, we can understand which devices (based on the Device’s declared capabilities) are eligible for use within a specific SmartApp.

Capabilities themselves are decomposed into both Commands and Attributes. Commands represent ways in which you can control or actuate the device, whereas Attributes represent state information or properties of the device.

Capabilities are created and maintained by the SmartThings internal development team.

This page serves as a reference for the supported capabilities.

.. note::

    This document is a work in progress.
    Many capabilities are not yet fully documented.
    We are continually working to document all the capabilities.

----

At a Glance
-----------

The Capabilities reference table below lists all capabilities. The various columns are:

Name:
    The name of the capability that is used by a Device Handler.
Preferences Reference:
    The string you would use in a SmartApp to allow a user to select from devices supporting this capability.
Attributes:
    The attributes that the capability defines.
Commands:
    The commands (and their signatures) that the capability defines.

{#- First iterate through the capabilities to create a quick reference table #}
{% for capability in capabilities %}
{%- set referenceName = capability['reference']['@name'] %}
{%- if loop.first %}
{{ '+' }}{{ '-' * 40 }}{{ '+' }}{{ '-' * 40 }}{{ '+' }}
{{ '|' }} Name{{ ' ' * 35 }}{{ '|' }} Preferences Reference{{ ' ' * 18 }}{{ '|' }}
{{ '+' }}{{ '=' * 40 }}{{ '+' }}{{ '=' * 40 }}{{ '+' }}
{% endif -%}
{{ '|' }} :ref:`{{ referenceName }}`{{ ' ' * (40 - (referenceName|length) - 8) }}{{ '|' }} {{ "capability."+referenceName }}{{ ' ' * (40 - (referenceName|length) - 12)}}{{ '|' }}
{{ '+' }}{{ '-' * 40 }}{{ '+' }}{{ '-' * 40 }}{{ '+' }}
{% endfor %}
----
{#- Generate detail section for each capability #}
{% for capability in capabilities %}
{% set referenceName = capability['reference']['@name'] %}
.. _{{ referenceName }}:
{# Capability name as section header #}
{{ capability['@name'] }}
{{ '-' * capability['@name']|length }}
{# Capability description #}
{{ properties[referenceName][referenceName+".description"] }}
{# Capability preference reference #}
Preferences Reference:
{{ ("capability."+referenceName)|indent(2, true) }}
{# Capability attributes section #}
**Attributes:**
{% if capability['attribute'] %}
{#- check if the attribute key in dict is a list. It will not be a list if there is only one attribute #}
{%- if capability['attribute'] is a_list %}
{#- for each attribute, print its name and type followed by its attribute value list if present #}
{%- for attribute in capability['attribute'] %}
  *{{ attribute['@name'] }}:* {{ attribute['@type'] }}
    {%- if attribute['value'] %}
    {%- if attribute['value'] is a_list %}
    {%- for value in attribute['value'] %}
    ``{{ value['@name'] }}``
    {%- endfor %}
    {%- else %}
    ``{{ attribute['value']['@name'] }}``
    {%- endif %}
    {%- else %}
    ``{{ properties[referenceName][referenceName+"."+attribute['@name']+".value"] }}``
    {%- endif %}
{%- endfor %}
{#- handle case if we only have one attribute and it wasn't a list in the dict #}
{%- else %}
{#- for this attribute, print its name and type followed by its attribute value list if present #}
  *{{ capability['attribute']['@name'] }}:* {{ capability['attribute']['@type'] }}
    {%- if capability['attribute']['value'] %}
    {%- if capability['attribute']['value'] is a_list %}
    {%- for value in capability['attribute']['value'] %}
    ``{{ value['@name'] }}``
    {%- endfor %}
    {%- else %}
    ``{{ capability['attribute']['value']['@name'] }}``
    {%- endif %}
    {%- else %}
    ``{{ properties[referenceName][referenceName+"."+capability['attribute']['@name']+".value"] }}``
    {%- endif %}
{%- endif %}
{%- else %}
  None
{%- endif %}

{# Capability commands section #}
**Commands:**
{% if capability['command'] %}
{#- check if the command key in dict is a list. It will not be a list if there is only one command #}
{%- if capability['command'] is a_list %}
{#- for each command, print its name method signature followed by its description #}
{%- for command in capability['command'] %}
  *{{ command['@name'] }}({% if command['argument'] %}{% if command['argument'] is a_list %}{% for arg in command['argument'] %}{{ arg['@type'] }} {{ arg['@name'] }}, {% endfor %}{% else %}{{ command['argument']['@type'] }} {{ command['argument']['@name'] }}{% endif %}{% endif %}):*
    {{ properties[referenceName][referenceName+"."+command['@name']+".description"] }}
{%- endfor %}
{#- handle case if we only have one command and it wasn't a list in the dict #}
{%- else %}
{#- for this command, print its name method signature followed by its description #}
  *{{ capability['command']['@name'] }}:*
    {{ properties[referenceName][referenceName+"."+capability['command']['@name']+".description"] }}
{%- endif %}
{%- else %}
  None
{%- endif %}
{% if not loop.last %}
----
{%- endif %}
{%- endfor %}

{#- These are attempts at table generation that are around for reference #}
{#- {%- if capability['attribute'] %} #}
{#- {%- if capability['attribute'] is a_list %} #}
{#-    {%- for attribute in capability['attribute'] %} #}
{#-       {%- if loop.first %} #}
{#-          {{ '+' }}{{ '-' * 30 }}{{ '+' }} #}
{#-          {{ '|' }} Name{{ ' ' * 25 }}{{ '|' }} #}
{#-          {{ '+' }}{{ '=' * 30 }}{{ '+' }} #}
{#-       {% endif -%} #}
{#-       {{ '|' }} {{ attribute['@name'] }}{{ ' ' * (30 - ((attribute['@name']|length) + 1)) }}{{ '|' }} #}
{#-       {{ '+' }}{{ '-' * 30 }}{{ '+' }} #}
{#-   {% endfor -%} #}
{#- {%- else %} #}
{#-    {{ '+' }}{{ '-' * 30 }}{{ '+' }} #}
{#-    {{ '|' }} Name{{ ' ' * 25 }}{{ '|' }} #}
{#-    {{ '+' }}{{ '=' * 30 }}{{ '+' }} #}
{#-    {{ '|' }} {{ capability['attribute']['@name'] }}{{ ' ' * (30 - ((capability['attribute']['@name']|length) + 1)) }}{{ '|' }} #}
{#-    {{ '+' }}{{ '-' * 30 }}{{ '+' }} #}
{#- {% endif -%} #}
{#- {%- else %} #}
{#- None #}
{#- {%- endif %} #}

{#- {% if capability['command'] %} #}
{#- {% if not capability['command']['@name'] %} #}
{#- {% for command in capability['command'] %}{% if loop.first %}{{ '+' }}{{ '-' * 20 }}{{ '+' }} #}
{#- {{ '|' }} Name{{ ' ' * 15 }}{{ '|' }} #}
{#- {{ '+' }}{{ '=' * 20 }}{{ '+' }}{% endif %} #}
{#- {{ '|' }} {{ command['@name'] }}{{ ' ' * (20 - ((command['@name']|length) + 1)) }}{{ '|' }} #}
{#- {{ '+' }}{{ '-' * 20 }}{{ '+' }}{% endfor %} #}
{#- {% else %}{{ capability['@name'] }} #}
{#- {% endif %} #}
{#- {% else %} #}
{#- None #}
{#- {% endif %} #}
