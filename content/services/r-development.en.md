---
title: "R development and automation"
slug: "r-development"
seoTitle: "R development, Shiny apps and automated reporting | datanalyze"
description: "R scripts, Shiny applications, packages and automated reports with Quarto. Turning a manual, fragile analysis into a reliable, reproducible tool."
order: 5
icon: "code"
serviceType: "R development and analysis automation"
tagline: "An analysis that runs itself, identically"
lead: "You redo the same manipulations every month, or you have inherited a script nobody dares touch. I turn those analyses into tools that rerun in one command and give the same result every time."

audienceIntro: "This service is for those whose analysis already exists, but costs too much to run, to fix or to hand over."
audience:
  - "Teams rebuilding the same report by hand every month"
  - "Researchers needing reproducible analyses to publish"
  - "Organisations that inherited undocumented code"
  - "R users wanting to make their scripts faster and safer"
  - "Labs wanting to distribute an internal tool"
  - "Method authors wanting to publish as a package"

problem:
  paragraphs:
    - "An analysis done by hand in a spreadsheet is correct exactly once: the day it was done. The following month a column has moved, a formula was not copied down, and the discrepancy goes unnoticed. The real cost is not the time spent, it is the lost confidence in the numbers."
    - "The opposite symptom is the inherited script: it works, nobody knows quite how, and every change is a gamble. Eventually the team routes around the tool rather than fixing it."
    - "In both cases the remedy is the same: make the processing explicit, tested and repeatable."
  signals:
    - "The monthly report takes two days of copy-paste"
    - "Nobody can reproduce last quarter's figures"
    - "A script works on one machine and not on another"
    - "An upstream correction forces you to redo everything"
    - "Your R code is slow enough to get in the way"
    - "You want to publish a tool, but not your code as it stands"

deliverablesIntro: "The goal is for the tool to outlive you: readable, documented and modifiable by someone other than its author."
deliverables:
  - "Structured, commented, version-controlled R scripts"
  - "Quarto or R Markdown reports that regenerate themselves"
  - "An R Shiny application to explore your data without coding"
  - "An installable, documented and tested R package"
  - "Taking over and rebuilding existing code"
  - "A handover session with your teams"

process:
  - title: "Audit"
    text: "A review of what exists: what works, what is fragile, what can go."
  - title: "Target"
    text: "We define together what the tool must do, and above all what it need not do."
  - title: "Development"
    text: "Built in deliverable stages, so you can test early rather than at the end."
  - title: "Handover"
    text: "Documentation, walkthrough and, if needed, training on maintenance."

examplesIntro: "Examples of typical work."
examples:
  - title: "Automated monthly report"
    text: "A Quarto document that reads the current data and regenerates text, tables and figures in one command."
  - title: "Shiny dashboard"
    text: "A web interface where your teams filter and explore the data without writing a line of code."
  - title: "Internal R package"
    text: "In-house functions scattered across several scripts, gathered into a documented and tested package."
  - title: "Taking over legacy code"
    text: "A script that became unreadable, rebuilt, verified against unchanged results and made modifiable again."
  - title: "Speeding up a computation"
    text: "A calculation that took hours brought down to minutes through vectorisation and parallelisation."
  - title: "Reproducible analysis"
    text: "A project where every figure and every number in the paper regenerates from the raw data."

faq:
  - q: "Does someone on our side need to know R?"
    a: |
      No. A Shiny application or an automated report is used without writing any
      code. If you want to extend the tool yourselves afterwards, though, you need at
      least one person comfortable with R — which is exactly what the
      [training](/en/trainings/) covers.
  - q: "Why R rather than Python or a BI tool?"
    a: |
      Because it is the tool I know best and the one best suited to work with a heavy
      statistical component. If your need is really about software engineering or
      data infrastructure, I will say so rather than stretching R beyond what it is
      good at.
  - q: "Can you take over code written by someone else?"
    a: |
      Yes, that is a frequent request. I start by checking that I reproduce the
      existing results exactly before changing anything — otherwise it becomes
      impossible to tell a fix from a regression.
  - q: "Where will the Shiny application be hosted?"
    a: |
      Depending on your constraints: on your own servers, on a Shiny hosting service,
      or locally if the data must not leave the building. We discuss it at scoping,
      because it drives some technical choices.
  - q: "Do I own the code?"
    a: |
      Entirely. Everything developed during the assignment is transferred to you, and
      to you only. You are free to modify it, distribute it or hand it to someone
      else.
---
