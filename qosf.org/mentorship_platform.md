---
layout: mentorship_platform
custom_css: mentorship_platform.css
keywords: quantum computing mentorship, QOSF mentorship, mentor matching, Jekyll application
comments: false
title: Mentorship Cohort Platform
description: A Jekyll-powered operations workspace for application intake, mentor matching, secure records, project timelines, and cohort execution.
buttons:
  - content: Mentee intake
    url: '#mentee-mentor-intake'
    icon: fa-user-graduate
  - content: Matching dashboard
    url: '#matching-workbench'
    icon: fa-project-diagram
  - content: Cohort execution
    url: '#execution-dashboard'
    icon: fa-tasks
---

{% assign platform = site.data.mentorship_platform %}

<span class="eyebrow">{{ platform.cohort.cohort_label }} · {{ platform.cohort.name }}</span>

This page is a markdown/Jekyll application design that uses the same QOSF site header, footer, typography, buttons, colors, and content pipeline as the main website. The public experience remains static and fast, while private application records are submitted to a secure API and database behind the Jekyll front end.

<div class="metrics-grid">
{% for metric in platform.metrics %}
  <div class="metric-card">
    <strong>{{ metric.value }}</strong>
    <span class="eyebrow">{{ metric.label }}</span>
    <p>{{ metric.note }}</p>
  </div>
{% endfor %}
</div>

## Product scope

<div class="platform-grid">
  <div class="platform-card">
    <h3>Application intake</h3>
    <p>Collect structured mentee and mentor records through theme-matched forms with consent, privacy notices, required-field validation, and confirmation emails.</p>
  </div>
  <div class="platform-card">
    <h3>Matching workbench</h3>
    <p>Rank mentee-mentor options using timezone, availability, educational level, research interests, capacity, and collaboration preferences.</p>
  </div>
  <div class="platform-card">
    <h3>Secure operations database</h3>
    <p>Persist all submitted data, review decisions, project milestones, access logs, and communication history in a private database.</p>
  </div>
  <div class="platform-card">
    <h3>Execution dashboard</h3>
    <p>Give organizers, mentors, and mentees clear access to cohort timelines, deadlines, project status, blockers, deliverables, and final showcase readiness.</p>
  </div>
</div>

## Mentee/mentor intake

<div id="mentee-mentor-intake" class="form-grid">
{% for form in platform.application_forms %}
  <div class="form-card">
    <span class="eyebrow">{{ form[0] }} form</span>
    <h3>{{ form[1].title }}</h3>
    <p>{{ form[1].description }}</p>
    <ul class="field-list">
    {% for field in form[1].fields %}
      <li>{{ field }}</li>
    {% endfor %}
    </ul>
  </div>
{% endfor %}
</div>

### Theme-matched intake form example

<form aria-label="Mentorship application example" method="post" action="/api/mentorship/applications">
  <label for="applicant-role">Applicant type</label>
  <select id="applicant-role" name="role" required>
    <option value="mentee">Mentee</option>
    <option value="mentor">Mentor</option>
  </select>

  <label for="applicant-timezone">Timezone</label>
  <input id="applicant-timezone" name="timezone" placeholder="UTC+01, UTC-05, Europe/Warsaw, etc." required>

  <label for="applicant-level">Educational or professional level</label>
  <select id="applicant-level" name="level" required>
    <option>Undergraduate</option>
    <option>Graduate student</option>
    <option>PhD / postdoc</option>
    <option>Industry professional</option>
    <option>Independent learner</option>
  </select>

  <label for="applicant-interests">Research interests</label>
  <textarea id="applicant-interests" name="interests" rows="4" placeholder="Quantum algorithms, quantum chemistry, error correction, QML, open-source tooling..." required></textarea>

  <div class="button-row">
    <button class="btn btn--dark btn--rounded" type="submit">Submit securely</button>
    <a class="btn btn--dark btn--rounded" href="#data-security">Review data controls</a>
  </div>
</form>

## Matching workbench

<div id="matching-workbench">
  <p>The platform should calculate an explainable compatibility score, then let organizers override or annotate recommendations before invitations are sent. Scores are advisory: human review is required for conflicts of interest, capacity constraints, and equitable distribution of opportunities.</p>

  {% for weight in platform.matching_weights %}
  <div class="weight-row">
    <span class="eyebrow">{{ weight.factor }} · {{ weight.weight }}%</span>
    <div class="weight-bar"><span style="width: {{ weight.weight }}%;"></span></div>
    <p>{{ weight.detail }}</p>
  </div>
  {% endfor %}

  <h3>Example recommendations</h3>
  {% for match in platform.sample_matches %}
  <div class="match-row">
    <p><span class="score-pill">{{ match.score }}%</span> <strong>{{ match.mentee }}</strong> → <strong>{{ match.mentor }}</strong></p>
    <p><strong>Timezone:</strong> {{ match.timezone }} · <strong>Interests:</strong> {{ match.interests }} · <strong>Status:</strong> {{ match.status }}</p>
  </div>
  {% endfor %}
</div>

## Secure data architecture

<div id="data-security" class="security-panel">
  <div class="architecture-flow" aria-label="Secure architecture flow">
    <div class="architecture-step">Jekyll UI</div>
    <div class="architecture-step">Authenticated API</div>
    <div class="architecture-step">Encrypted database</div>
    <div class="architecture-step">Organizer dashboard</div>
    <div class="architecture-step">Audited exports</div>
  </div>

  <ul class="security-list">
  {% for control in platform.security_controls %}
    <li>{{ control }}</li>
  {% endfor %}
  </ul>
</div>

## Execution dashboard

<div id="execution-dashboard">
  <p><strong>Application window:</strong> {{ platform.cohort.application_window }} · <strong>Program window:</strong> {{ platform.cohort.program_window }} · <strong>Decision release:</strong> {{ platform.cohort.decision_release }}</p>

  <div class="timeline">
  {% for item in platform.program_timeline %}
    <div class="timeline-item">
      <span class="eyebrow">{{ item.dates }} · {{ item.owner }}</span>
      <h3>{{ item.phase }}</h3>
      <p>{{ item.output }}</p>
    </div>
  {% endfor %}
  </div>

  <h3>Project status board</h3>
  <div class="status-grid">
  {% for project in platform.projects %}
    <div class="project-card">
      <span class="health-pill health-{{ project.health }}">{{ project.status }}</span>
      <h3>{{ project.title }}</h3>
      <p><strong>Mentor:</strong> {{ project.mentor }} · <strong>Mentees:</strong> {{ project.mentees }}</p>
      <p><strong>Next milestone:</strong> {{ project.milestone }}</p>
      <p><strong>Due:</strong> {{ project.due }}</p>
    </div>
  {% endfor %}
  </div>
</div>

## Markdown/Jekyll operating model

- Program content, cohort dates, matching weights, and project statuses live in `_data/mentorship_platform.yml` so non-developers can update the cohort workspace with pull requests.
- This page is authored in Markdown with Liquid loops, which keeps the implementation compatible with the existing Jekyll site.
- Sensitive data is never committed to the static repository; forms submit to a private API and database, while the public site only stores schema, documentation, and non-sensitive operational summaries.
- The interface uses `layout: mentorship_platform`, QOSF's existing includes, and `assets/css/mentorship_platform.css` to preserve the exact QOSF theme while allowing a wider dashboard canvas.
