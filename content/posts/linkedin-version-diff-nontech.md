---
title: "Designing AI‑assisted project updates that people actually trust"
date: 2026-03-09T00:00:00Z
publishDate: 2026-03-10T09:00:00Z
draft: false
---

Most project tools now offer some version of “Let AI rewrite this update.” You click it, a new wall of text appears, and you’re left wondering what, exactly, just changed.

On my own project board, I didn’t want AI to take over status updates or project descriptions. I wanted it to handle some of the writing, without taking away control of the message.

So instead of a single “rewrite everything” button, I designed a review flow where AI suggests small, targeted edits that you can accept or ignore one field at a time.

The real problem: invisible changes
On a typical “Edit project” page you might have:

- A title and description
- A status and priority
- A list of subtasks or tickets

The most obvious way to add AI looks like this:

1. Send the whole project to the model with an instruction like “make this clearer.”
2. Get back a new version.
3. Replace the existing content.

On paper that sounds efficient. In practice it breaks trust.

- You can’t see exactly what changed.
- You might like the new description but dislike that the status or tone quietly shifted.
- If something feels off, there’s no easy way to undo only part of it.

For product managers and leads, that’s not a small risk. An AI “improvement” might accidentally soften a risk, over‑promise on a date, or change the message to stakeholders without anyone spotting it.

The mental model: AI proposes, humans approve
The shift that made this usable was straightforward:

- The form stays the single source of truth.
- The AI never edits it directly.
- Instead, the AI returns a set of suggested changes.

Think of it like a junior teammate working in suggestion mode. Nothing is final until you approve it.

For every field on the page (title, description, status, each subtask), I keep three versions in mind:

- What was originally saved.
- What you’ve manually typed so far.
- What the AI is proposing, if anything.

The interface’s job is to show those differences clearly, not to hide them.

How the review flow works in practice
Here’s what a typical interaction looks like from a product owner or lead’s point of view.

1. You open a project to edit it.

   You see the same form you’re used to: title, description, status, subtasks. Nothing feels different yet.

2. You ask the built‑in assistant for help.

   In a side panel, you might write:

   - “Tighten this description for an exec update.”
   - “Clarify the acceptance criteria.”
   - “Break this into 3-4 subtasks.”

   Behind the scenes, the system sends the current project plus your request to the model, and gets back suggested edits for specific fields.

3. Fields with suggestions light up.

   Only the fields that actually have proposals show a small “View AI suggestion” link next to their label.

   - If there’s no suggestion for status, that field stays completely normal.
   - If there *is* a suggestion for the description, you’ll see the affordance there and nowhere else.

4. You review one field at a time.

   When you click “View AI suggestion” on, say, the description:

   - You see your current text on one side.
   - You see the AI’s version on the other.
   - In between, Can you see a compact view of what actually changed? New sentences, removed sentences, wording tweaks.

   At the bottom you can:

   - Apply the AI suggestion for this field only.
   - Keep your current version.
   - Close the window and ignore it for now.

5. You save when you’re happy.

   When you finally hit “Save project”, only the values in the form are stored. The AI’s proposals never become “the truth” on their own.

In other words, AI can suggest, but you decide what reaches your team or stakeholders.

Handling subtasks and tickets without chaos
Projects usually involve more than a single description. They might include:

- Subtasks with titles and descriptions
- Linked tickets
- Notes or comments

To keep this manageable, I kept suggestions simple in those areas:

- For short text (like subtask titles), I show a one‑line before/after instead of a dense diff.
- For longer text (like subtask descriptions), I reuse the same focused diff view as the main description.
- When the AI suggests adding or removing a subtask, it shows a clear badge like “AI suggested a new subtask” with accept/reject buttons.

The rule doesn’t change: the AI can’t silently restructure your project. Every change is visible and optional.

Why this works better for non‑technical teams
After using this flow on my own projects for a while, a few patterns showed up that matter to product and leadership roles.

**You keep control of the message.**  
The assistant can help make updates clearer or more concise, but you always see what’s changing before it goes out.

**You can say yes or no in small pieces.**  
You might accept the improved wording in the description and reject a risky change to status or scope. Each field is its own decision.

**It’s safe to experiment.**  
Because nothing is committed until you hit “Save”, people feel free to try suggestions without worrying they’ll accidentally ship something wrong.

**The pattern scales to other parts of the tool.**  
Once this “AI proposes, you review” pattern exists for projects, it can extend to tickets, release notes, customer emails, and more, without redesigning everything from scratch.

How you might apply this in your product
If you own a product or team that’s adding AI assistance around content, a few principles from this approach are worth considering:

- Treat AI as a suggestion engine, not an auto‑pilot. The moment it starts changing things silently, people stop trusting it.
- Keep one obvious place where truth lives. In this case, it’s the form on the screen; everything else is just input.
- Design for “no” as much as for “yes”. Accepting, rejecting, or ignoring a suggestion should all feel equally easy.
- Show the change, not just the result. A small, clear diff builds more confidence than a mysterious new paragraph.

AI can absolutely help with project updates, roadmaps, and stakeholder comms. The goal is to design it so that people feel *more* in control with it turned on, not less.

