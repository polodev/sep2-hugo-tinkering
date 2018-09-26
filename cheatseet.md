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
    │   └── index.md
    ├── basic-html-2
    │   └── index.md
    └── basic-html-3
        └── index.md

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

# set taxonomies in config.toml file
[taxonomies]
  category="categories"
  tag="tags"
  authors="authors"
  course='courses'
  singular='plural'

# basic taxonomy iteration for menu

~~~html
{{ range $key, $value := .Site.Taxonomies.dtags }}
<li class="list-group-item">
  <a class="btn btn-link" href="{{$.Site.BaseURL}}/dtags/{{$key | urlize}}">{{ $key | humanize }}</a> ({{len $value }})
</li>
{{ end }}
~~~


# showing content form same series 
~~~bash
<ul>
    {{ range .Site.Taxonomies.series.golang }}
        <li><a href="{{ .Page.RelPermalink }}">{{ .Page.Title }}</a></li>
    {{ end }}
</ul>
~~~
~~~bash
it could be reside following layouts in my case
layout > taxonomy > series > list + single
~~~

# showing all course with nested post 

~~~bash
<h2>All courses with nested course topics</h2>
<section id="menu">
  <ul>
      {{ range $key, $taxonomy := .Site.Taxonomies.courses }}
      <li>{{ $key }}</li>
      <ul>
          {{ range $taxonomy.Pages }}
          <li data-group="{{ .Params.group_name }}" hugo-nav="{{ .RelPermalink}}"><a href="{{ .Permalink}}">{{ .LinkTitle }}</a></li>
          {{ end }}
      </ul>
      {{ end }}
  </ul>
</section>
~~~

# GroupByParam

~~~bash
<section id="menu">
  {{ range .Pages.GroupByParam "group_name" }}
    <h3>{{ .Key }}</h3>
    <ul>
        {{ range .Pages }}
        <li>
        <a href="{{ .Permalink }}">{{ .Title }}</a>
        <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
        </li>
        {{ end }}
    </ul>
  {{ end }}
</section>
~~~


# standalone taxonomy page
~~~bash
taxonomy/tag.html
taxonomy/category.html
taxonomy/course.html
=================

{{ partial "header.html" . }}
<section id="main">
  <div class="container">
   <h1 id="title">{{ .Title }}</h1>
    {{ range .Data.Pages }}
        <ul>
            <li><a href="{{ .Permalink }}">{{ .Title }}</a></li>
        </ul>
    {{ end }}
  </div>
</section>

{{ partial "footer.html" . }}
~~~


# all-taxonomies-keys-and-pages.html

~~~bash
<section>
  <ul>
    {{ range $taxonomyname, $taxonomy := .Site.Taxonomies }}
      <li><a href="{{ "/" | relLangURL}}{{ $taxonomyname | urlize }}">{{ $taxonomyname }}</a>
        <ul>
          {{ range $key, $value := $taxonomy }}
          <li> {{ $key }} </li>
                <ul>
                {{ range $value.Pages }}
                    <li><a href="{{ .Permalink}}"> {{ .LinkTitle }} </a> </li>
                {{ end }}
                </ul>
          {{ end }}
        </ul>
      </li>
    {{ end }}
  </ul>
</section>
~~~


# showing all posts by grouping in single post, where current single post also has same category
~~~bash
{{range $key, $taxonomy := $.Site.Taxonomies.courses}}
  {{if eq $key $course_name}}
    <section id="menu">
      {{ range $taxonomy.Pages.GroupByParam "group_name" }}
        <h3>{{ .Key }}</h3>
        <ul>
            {{ range .Pages.ByWeight }}
            <li>
              <a href="{{ .Permalink }}">{{ .Title }}</a>
              <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
            </li>
            {{ end }}
        </ul>

      {{ end }}
    </section>

  {{end}}
{{end}}
~~~

# in case of multiple categories plus its safe  // it will be my menu

~~~bash
{{$post_courses := .Params.courses }}
<h2>Test 3</h2>
{{range $taxonomy_name, $taxonomy := $.Site.Taxonomies.courses}}
  {{if in $post_courses $taxonomy_name}}
    <section id="menu">
      {{ range $taxonomy.Pages.GroupByParam "group_name" }}
        <h3>{{ .Key }}</h3>
        <ul>
            {{ range .Pages.ByWeight }}
            <li>
              <a href="{{ .Permalink }}">{{ .Title }}</a>
              <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
            </li>
            {{ end }}
        </ul>

      {{ end }}
    </section>

  {{end}}
{{end}}

~~~


# if course doesn't have grouping item - added if else statement 
~~~bash
<h2>Menu</h2>
{{range $taxonomy_key, $taxonomy := $.Site.Taxonomies.courses}}
  {{if in $post_courses $taxonomy_key}}
    <section id="menu">
      {{if $post_group_name }}
        {{ range $taxonomy.Pages.GroupByParam "group_name" }}
          <h3>{{ .Key }}</h3>
          <ul>
              {{ range .Pages.ByWeight }}
              <li>
                <a href="{{ .Permalink }}">{{ .Title }}</a>
                <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
              </li>
              {{ end }}
          </ul>
        {{ end }}
     {{ else }} 
        <ul>
            {{ range $taxonomy.Pages.ByWeight }}
            <li>
              <a href="{{ .Permalink }}">{{ .Title }}</a>
              <div class="meta">{{ .Date.Format "Mon, Jan 2, 2006" }}</div>
            </li>
            {{ end }}
        </ul>
      {{end}}
    </section>
  {{end}}
{{end}}
~~~