---
layout: post
title: "Bringing Conductor to My Phone"
date: 2026-03-30
---

[Conductor](https://docs.conductor.build/) is not one of those tools that looks impressive in a demo but never makes it into your actual workflow. It's something I genuinely use every day: spinning up branches, switching context between tasks, running multiple agents in parallel across different workspaces, or picking up where I left off in an existing session. It's become the interface I reach for first and rely on most.

A lot of the time, I'm not even thinking of it as "using an AI tool." It's more like a different way of working: break the task apart, pin each piece of context to its own workspace, let different sessions make progress independently, then come back and pull the results together. Or just let it run multiple my todo list at the same time. Conductor makes that flow smooth enough that it just became my default.

But precisely because I use it so much, one gap kept getting more obvious: the moment I step away from my computer, Conductor is basically unreachable.

That's not a flaw in Conductor. It might be by design. It's a local workbench, tightly coupled to the repos, workspaces, and session state on my machine. The problem is that I don't sit at my desk all day. I go out, grab lunch, walk around, or just step away for a bit. My tasks don't pause when I do. I'd want to glance at which workspace a branch is in, check how far a session got, see if there's anything in the queue, or even fire off a quick follow-up instruction.

So I built this: a Telegram bridge for Conductor that runs on the same Mac. It's not "Conductor in the cloud," and it's not a separate remote agent system. It's just a mobile entry point into the same local Conductor instance, letting me keep working from Telegram on my phone.

The project is here: [conductor_mobile](https://github.com/xudong963/conductor_mobile).

## What it supports today

- Browse repos and switch between them
- Browse branches and workspaces, see their descriptions, and switch context
- Browse chats and sessions and continue an existing conversation
- Start a new Conductor chat on the current branch
- Create a new workspace in the current repo
- Bind individual Conductor chats to separate Telegram topics in topic-enabled groups
- Stream agent text output back to Telegram in real time
- View the current session's queue status
- Page through session context in a single message
- Handle basic `requestUserInput` and plan feedback flows
- Auto-sync Telegram slash commands so there's less to memorize

To be upfront, this is still very much a scratching-my-own-itch project. It's single-user, Codex-session only, and I haven't tried to tackle multi-user auth, multi-model support, or anything like that yet.

One thing I do like about it is that it doesn't invent new abstractions. Repos, workspaces, and sessions all map directly to the same Conductor concepts. The switching cost is near zero. I'm not learning some mobile version of the mental model; I'm just accessing the same workflow from a different door.

## The boundaries

That said, there are clear boundaries.

First, the bridge has to run on the same Mac where Conductor is installed and initialized. It reads directly from the local Conductor database and invokes the Codex binary on that machine. So it's not remote-controlling another computer. It's adding a remote entry point to this computer.

Second, if I want it to stay available while I'm away from my desk, I need to register the process as a `LaunchAgent` via `launchd`. That way, even when I lock the screen or let the display turn off, the process stays alive and Telegram keeps receiving messages.

But there's one hard constraint: ~~**the laptop lid has to stay open.**~~

The reason is straightforward. `launchd` handles keeping the process running after login, but it can't prevent the machine from sleeping when you close the lid. Once the Mac actually sleeps, everything stops: the bridge, Conductor, and the local environment it depends on. So the practical setup is still to keep the Mac plugged in and logged in. Screen off is fine, and if I want to close the lid without taking the whole setup offline, Amphetamine can help keep the Mac awake.

In other words, this is more of an away-from-desk extension panel than a fully cloud-independent service.

## Why it matters to me

Even so, it's already been genuinely useful to me. Most of the time, what I need isn't to do all my development from my phone. It's to keep managing and pushing forward the tasks that are already running on my computer while I'm not sitting in front of it. Being able to switch workspaces, check context, continue a session, or process the queue means a lot of otherwise dead time gets reclaimed.

That's what I like about this project: it's not trying to be a bigger, more complete system. It's just filling the one gap I kept running into in my daily use of Conductor.

If I keep working on it, the priority will be making it a more stable and natural remote entry point, not piling on flashy features. For a tool like this, reliability, staying online, and keeping context straight matter more than anything.

At the end of the day, it solves a very concrete problem for me: now, even when I'm away from my desk, it's no longer completely out of reach.
