---
citekey: "{{ citekey }}"
aliases:
  - "{{ title }}"
tags:
  - source
topics: "[[!{{citekey}}]]"
readed: false
---

> [!cite] Cite
> {{bibliography}}

> [!note] Synthesis
> **Summary**:: %%brief description of the source content%%
> **Contribution**:: %%key intellectual value%%
> **Related**:: {% for relation in relations | selectattr("citekey") %} [[@{{relation.citekey}}]]{% if not loop.last %}, {% endif%} {% endfor %}

> [!example] Metadata
{%- if creators.length > 0 %}
{% for type, creators in creators | groupby("creatorType") -%}
{%- for creator in creators -%}
> **{{"First" if loop.first}}{{type | capitalize}}**::
{%- if creator.name %} {{creator.name}}
{%- else %} {{creator.lastName}}, {{creator.firstName}}
{%- endif %}
{% endfor %}~
{%- endfor %}
{%- else %}
> **Creator Unidentified**
{%- endif %}
> **Title**:: {{title}}
> **Year**:: {{date | format("YYYY")}}
> **Citekey**:: {{citekey}}
{%- if itemType %}
> **ItemType**:: {{itemType}}
{%- endif %}
{%- if itemType == "journalArticle" %}
> **Journal**:: *{{publicationTitle}}*
{%- endif %}
{%- if volume %}
> **Volume**:: {{volume}}
{%- endif %}
{%- if issue %}
> **Issue**:: {{issue}}
{%- endif %}
{%- if itemType == "bookSection" %}
> **Book**:: {{publicationTitle}}
{%- endif %}
{%- if publisher %}
> **Publisher**:: {{publisher}}
{%- endif %}
{%- if place %}
> **Location**:: {{place}}
{%- endif %}
{%- if pages %}
> **Pages**:: {{pages}}
{%- endif %}
{%- if DOI %}
> **DOI**:: {{DOI}}
{%- endif %}
{%- if ISBN %}
> **ISBN**:: {{ISBN}}
{%- endif %}
{%- if allTags %}
> **Keywords**:: {{ allTags }}
{%- endif %}
> **Category**:: literature-note
> **Source**:: zotero
> **Created**:: {{ exportDate | format("YYYY-MM-DD HH:mm:ss") }}

> [!info] [Zotero Link]({{desktopURI}})

> [!abstract] Abstract Note
> {{abstractNote}}

## Annotations
{%- macro calloutHeader(type, color) %}
{%- if type == "highlight" %} 
> [!quote|{{color}}] {{type}}:
{%- else %}
{%- if type == "note" %}
> [!note] {{type}}:
{%- else %}
> [!abstract] {{type}}:
{%- endif %}
{%- endif %}
{%- endmacro %}

{% persist "annotations" %}
{% set newAnnotations = annotations | filterby("date", "dateafter", lastImportDate) %}
{% if newAnnotations.length > 0 %}

### Imported: {{importDate | format("YYYY-MM-DD HH:mm:ss")}}

{% for annotation in newAnnotations %}
{{calloutHeader(annotation.type, annotation.color)}}
> 
> {{annotation.annotatedText}}
{%- if annotation.imageRelativePath %}
>
> ![[{{annotation.imageRelativePath}}|300]]
{%- endif %}
{%- if annotation.comment %}
>
>> {{annotation.comment}}
{%- endif %}
{%- if annotation.desktopURI %}
>
> ([p.{{annotation.pageLabel}}]({{annotation.desktopURI}}))
{%- endif %}
{% endfor %}
{%- endif %}
{% endpersist %}


## .template-information
%% #TODO разобраться с работой плагина 'Templater', сейчас при повторном применении шаблона к заметке команда отображается в заметке как текст %%

%%File creation with template%%
<%*
const dateFormat = "YYYY-MM-DD HH:mm"
if (tp.file.creation_date(dateFormat) === tp.date.now(dateFormat)) {
const source_notes = "!{{citekey}}"
await tp.file.create_new(tp.file.find_tfile("Source Notes Template"), source_notes)
}
-%>