---
uid: 599
title: Utility functions for udp / json-parser
date: 2013-05-09T00:16:51+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=599
categories:
  - Software Development
tags:
  - C
  - dump
  - json
  - json-parser
  - parse
  - plain C
comments: true
excerpt: Extend udp / json-parser functionality by adding json edition capabilities
---
Recently, I needed a json parser for a small C project, and -after googling- found this <a href="https://github.com/udp/json-parser" title="udp / json-parser" target="_blank">great library udp / json-parser</a>.

It is great, because it does what it says and it&#8217;s really low-footprint.

Soon I realized that I needed two things for my project: a) modifying a string on the json and b) dumping an already parsed json to a file. So, probably json-parser was not the right choice for my project and since I don&#8217;t have time to change it now, I decided to implement these functions myself.

Feel free to use, modify or fix as you need.

<!--more-->

**Disclaimer:** note that this code is not 100% ANSI C (specially the json\_dump\_file function), since my project will only run on Windows you might find crazy things like TCHAR, etc&#8230; but it shouldn&#8217;t be that difficult to make it ANSI.

{% highlight cpp %}
/**
 * Adds a key / value pair to an existing json_object node
 * @param  settings	pointer to settings
 * @param  obj		the parent object to add the key / value
 * @param  name		the key
 * @param  val		the value
 */
void json_add_object_value_ex (json_settings * settings, json_value * obj, json_char * name, json_value * val)
{
	unsigned int i, values_size;
	json_value *values;

	if (obj->type != json_object) {
		// unexpected type!
		return;
	}
	// check if the value already exists
	for (i = 0; i < obj->u.object.length; i++) {
		if (!strcmp(obj->u.object.values[i].name, name)) {
			// in this case just need to change the value
			json_value_free_ex (settings, obj->u.object.values[i].value);
			obj->u.object.values[i].value = val;
			val->parent = obj;
			return;
		}
	}
	// need to realloc the object values array
	// increment array length
	obj->u.object.length++;
	values_size = obj->u.object.length * sizeof (*obj->u.object.values);
	values = (json_value *)settings->mem_alloc(values_size, 0, settings->user_data);
	memcpy(values, obj->u.object.values, (obj->u.object.length - 1) * sizeof (*obj->u.object.values));
	*(void **)obj->u.object.values = &values;
	// set the name and value on the last element of the array
	obj->u.object.values[obj->u.object.length-1].name = (json_char *) settings->mem_alloc(sizeof(json_char) * strlen(name), 0, settings->user_data);
	strcpy_s(obj->u.object.values[obj->u.object.length-1].name, sizeof(json_char) * strlen(name), name);
	obj->u.object.values[obj->u.object.length-1].value = val;
	val->parent = obj;
}

/**
 * Shortcut to json_add_object_value_ex using default memory allocation functions
 */
void json_add_object_value (json_value * obj, json_char * name, json_value * val)
{
	json_settings settings = { 0 };
	memset(&settings, '\0', sizeof(json_settings));
	settings.mem_free = default_free;
	settings.mem_alloc = default_alloc;
	json_add_object_value_ex (&settings, obj, name, val);
}

/**
 * Allocate a json_string
 * @param  settings	pointer to settings
 * @param  string	the string to allocate
 * @return a json_value of type json_string with the string on it
 */
json_value * json_alloc_string_ex (json_settings * settings, json_char * string)
{
	json_value * val;
	int len = strlen(string);

	val = (json_value *)settings->mem_alloc(sizeof(json_value), 0, settings->user_data);
	val->type = json_string;
	val->u.string.length = len;
	val->u.string.ptr = (json_char *)settings->mem_alloc(len + 1, 0, settings->user_data);
	strcpy_s(val->u.string.ptr, len + 1, string);
	return val;
}

/**
 * Shortcut to json_alloc_string_ex using default memory allocation functions
 */
json_value * json_alloc_string (json_char * string)
{
	json_settings settings = { 0 };
	memset(&settings, '\0', sizeof(json_settings));
	settings.mem_free = default_free;
	settings.mem_alloc = default_alloc;
	return json_alloc_string_ex (&settings, string);
}

static void json_dump_escaped(FILE *f, char *buf, size_t len)
{
	size_t	i;

	for (i=0; i < len; i++) {
		switch(*buf) {
		case '\\': fprintf(f, "%s", "\\\\"); break;
		case '"':  fprintf(f, "%s", "\\\""); break;
		case '/':  fprintf(f, "%s", "\\/"); break;
		case '\b': fprintf(f, "%s", "\\b"); break;
		case '\f': fprintf(f, "%s", "\\f"); break;
		case '\n': fprintf(f, "%s", "\\n"); break;
		case '\r': fprintf(f, "%s", "\\r"); break;
		case '\t': fprintf(f, "%s", "\\t"); break;
		default: fputc(*buf, f); break;
		}
		buf++;
	}
}

static void json_dump_helper(FILE *f, json_value *v, int prettyprint, unsigned int level)
{
	unsigned int		i, j;

	switch(v->type) {
	case json_object:
		fputc('{', f);
		if (prettyprint) {
			if (v->u.object.length > 0) fprintf(f, "\n");
		}
		for (i=0; i < v->u.object.length; i++) {
			for(j=0; j < level; j++) fputc('\t', f);
			if (prettyprint) fputc('\t', f);
			fputc('"', f);
			json_dump_escaped(f, v->u.object.values[i].name, strlen(v->u.object.values[i].name));
			fprintf(f, "\":%s", (prettyprint) ? " "  : "");
			json_dump_helper(f, v->u.object.values[i].value, prettyprint, level + 1);
			if (i < (v->u.object.length - 1)) {
				fputc(',', f);
				if (prettyprint) fputc('\n', f);
			}
		}
		if (prettyprint) {
			fputc('\n', f);
			for(j=0; j < level; j++) fputc('\t', f);
		}
		fputc('}', f);
		break;
	case json_array:
		fputc('[', f);
		for (i=0; i < v->u.array.length; i++) {
			json_dump_helper(f, v->u.array.values[i], prettyprint, level + 1);
			if (i < (v->u.array.length - 1)) {
				fputc(',', f);
				if (prettyprint) fputc(' ', f);
			}
		}
		fputc(']', f);
		break;
	case json_integer:
		fprintf(f, "%lld", v->u.integer);
		break;
	case json_double:
		fprintf(f, "%f", v->u.dbl);
		break;
	case json_string:
		fputc('"', f);
		json_dump_escaped(f, v->u.string.ptr, v->u.string.length);
		fputc('"', f);
		break;
	case json_boolean:
		fprintf(f, "%s", (v->u.boolean) ? "true" : "false");
		break;
	case json_null:
		fprintf(f, "null");
		break;
	}
}

/**
 * Dumps a json object into a file
 */
int json_dump_file(TCHAR *fname, json_value *json, int prettyprint)
{
	FILE		*f;
	errno_t		rt = _tfopen_s(&f, fname, _T("wb+"));

	if (!rt) {
		json_dump_helper(f, json, prettyprint, 0);
		fclose(f);
		return 0;
	}
	return 1;
}
{% endhighlight %}
