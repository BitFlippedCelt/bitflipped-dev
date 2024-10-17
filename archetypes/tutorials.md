---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
categories:
- Tutorial
{{ range split (path.Base (.Dir) | title) "-" -}}
- {{ . }}
{{ end -}}
tags:
{{ range split (path.Base (.Dir) | lower) "-" -}}
- {{ . }}
{{ end -}}
---

# Headline

Amazing content goes here