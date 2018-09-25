# menu will be like gohugo like in front matter

# adding custom metadata for hugo taxonomy
https://gohugo.io/content-management/taxonomies/
~~~
/content/<TAXONOMY>/<TERM>/_index.m
~~~


# nested content structure

~~~bash
html
├── advanced-html
│   ├── advanced-html-1
│   │   ├── hugo-logo.png
│   │   └── index.md
│   ├── advanced-html-2
│   │   └── index.md
│   └── advanced-html-3
│       └── index.md
└── basic-html
    ├── basic-html-1
    │   └── _index.md
    ├── basic-html-2
    │   └── _index.md
    └── basic-html-3
        └── _index.md

~~~



# announcing a block and using it

~~~bash
{{ block "main" . }}{{ end }}
~~~

~~~bash
{{define "main"}}
<<content>>
{{end}}
~~~



# taxonomy pagination   

https://discourse.gohugo.io/t/using-pagination-with-terms/7947
~~~php
{{ range  .Paginator.Pages }}

{{ .Title }}: {{ .RelPermalink }}
{{ end }}
~~~




