---
layout: schedule
permalink: /lectures/
title: Schedule
---
Lectures slides, notes and annotations will appear <a href="https://umass-my.sharepoint.com/:f:/g/personal/smaji_umass_edu/Enj1cgQKn1tAopTl2EqJZ38B9ppAiCpjKvWz8I6dAh0J1w?e=f3JtOv">in this folder</a> (UMass credentials required). <br>
Links to the Echo360 will be posted on Piazza two weeks after each lecture. For early access submit the request form on Piazza.

{% assign current_module = 0 %}
{% assign skip_classes = 0 %}
{% assign prev_date = 0 %}

{% for item in site.data.lectures %}
{% if item.date %}
{% assign lecture = item %}
{% assign event_type = "upcoming" %}
{% assign today_date = "now" | date: "%s" | divided_by: 86400 %}
{% assign lecture_date = lecture.date | date: "%s" | divided_by: 86400 %}
{% if today_date > lecture_date %}
    {% assign event_type = "past" %}
{% elsif today_date <= lecture_date and today_date > prev_date %}
    {% assign event_type = "warning" %}
{% endif %}
{% assign prev_date = lecture_date %}

<tr class="{{ event_type }}">
    <th scope="row">{{ lecture.date }}</th>
    {% if lecture.title contains 'No class' or lecture.title contains 'review' or lecture.title contains 'exam' or forloop.last %}
    {% assign skip_classes = skip_classes | plus: 1 %}
    <td colspan="4" align="center">{{ lecture.title }}</td>
    {% else %}
    <td>
        Lecture #{{ forloop.index | minus: current_module | minus: skip_classes }}
        {% if lecture.lecturer %}({{ lecture.lecturer }}){% endif %}:
        <br />
        {{ lecture.title }}
    </td>
    <td>
        {% if lecture.readings %}
        <ul>
        {% for reading in lecture.readings %}
            <li>{{ reading }}</li>
        {% endfor %}
        </ul>
        {% endif %}
    </td>
    <td>
        <p>{{ lecture.logistics }}</p>
    </td>
    {% endif %}
</tr>
{% else %}
{% assign current_module = current_module | plus: 1 %}
{% assign module = item %}
<tr class="info">
    <td colspan="5" align="center"><strong>{{ module.title }}</strong></td>
</tr>
{% endif %}
{% endfor %}
