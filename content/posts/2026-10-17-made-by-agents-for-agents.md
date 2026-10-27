+++
title = 'made by agents, for agents'
date = 2026-10-17T00:00:00+05:30
draft = false
+++

Hey there, so- this one's been sitting in my head for a while now. Which is funny, because the whole thing is about forgetting, and I kept forgetting to write it.

Have you ever actually sat down and opened a codebase you built? Not skimmed it. Opened it. Tried to read it the way you'd read someone else's. Because I did that recently, and I couldn't.

And not the one I inherited. Mine. The one I architected, directed, signed off on. I couldn't tell you what's in the folders I made.

## The One I Inherited, and the One I Built

There's this codebase someone else "agentic engineered". Start to finish, done by agents. A handful of services, infra on top, the whole thing. I came into it later and I could not move through it.

Not "it took me a minute." I mean I could not navigate it. Not the bug. Not the feature. Not even finding the folder I needed. At one point I tried to explain it to someone and just couldn't. All of it, at once.

Then there's mine. The one I built. I architected it, every moving part went through me, I directed the whole thing. I know the flow. The general overview, what talks to what. The hard code, not anymore. And I can't put my finger on where any of it lives. I can't remember the folder structure. Ask me where anything is and I have to go and look. I haven't run a makefile command in it. Not once. Why would I? I got agents.

That's the part that gets me. I didn't inherit the confusion. I made it.

## The Creator Doesn't Know Either

I've asked the person who built theirs what it does. More than once. They don't know. Not fully. They'll pull up the agent and ask it, same as me.

And I've done it on mine. Someone asks me about my own project and I go ask Claude or Codex what it does, because apparently that's faster than knowing.

So who answers the question. When the person who built the thing is unsure how the thing works, who's left to ask.

## The Names Stopped Mattering

We used to know which file was what. Where the abstraction class lived. The whole hierarchy, top to bottom. You could ask someone a question about their codebase and they'd answer it out of their head.

Now the instruction is "add x". That's it. Nobody asks what the file is called. Nobody asks where the class goes. It gets added, it works, everyone moves on.

I don't think that's the devs. I think it's the shape of the work now, maybe the product side, the "just ship the task" of it all. Perfectionism going away slowly, and I can watch it happen, and I can't even be mad at anyone for it.

## Senior Devs Are Juniors Now

There used to be this thing. Tribal knowledge. A senior sits you down, tells you which file to look at, walks you through the flow, explains why it was built that way. You learned the codebase from someone who actually carried it in their head. That was the transfer. That was the whole job.

That's gone. And here's the part that actually gets me: the seniors don't have it either. They're asking Claude and Codex the same questions the juniors are. Same chat. Same time. The knowledge didn't move down the chain, it went somewhere none of us can reach.

Good for the juniors, sure. They can move without waiting on anybody. But who do they become. Who's the senior, if nobody's carrying it.

How did we even get here. Like how. Fucking how.

## The Pace Is the Thing

Here's how. It wasn't like this before because we used to be slow.

And the reason is boring: I never had time to care about it. Something that used to take months takes days now, sometimes weeks, and then it's another task done, another issue closed, on to the next one.

You'd write something. You'd read it again. You'd come back to it. You'd debug it for hours and learn it in the process. The slowness was the learning. That's how you ended up knowing your own codebase.

Say it's a function like this one.

```go
func BuildOrderSummary(ctx context.Context, userID string) (*Summary, error) {
	user, err := users.Get(ctx, userID)
	if err != nil {
		return nil, fmt.Errorf("get user %s: %w", userID, err)
	}

	orders, err := orders.ListByUser(ctx, user.ID)
	if err != nil {
		return nil, fmt.Errorf("list orders for %s: %w", userID, err)
	}

	total := money.Zero(user.Currency)
	for _, order := range orders {
		total = total.Add(order.Total)
	}

	return &Summary{User: user, Count: len(orders), Total: total}, nil
}
```

Nothing special. Calls two services, adds the totals, returns.

Now remember what it took to know where that lived. You'd get the error, go looking, jump to a definition and land somewhere else. Split the window so you could see the caller and the function at the same time, navigate both, run it in the terminal, get another error, go again. Somewhere in there the file clicked. You got reminded of the function, of what sat next to it. You knew where to look because you'd been there enough times.

That's the part I miss. Not the code. Knowing where it is.

Now it grows faster than you can read it. Features every day. Whole services appearing. Feels like there's a hundred thousand PRs landing daily across the industry, maybe more, I don't know the exact number. But that's the feeling. It's not landing in your head, it's landing in the repo.

You can't keep up with a thing that moves faster than you can read. So you stop reading. And then you forget.

You don't build a feel for a thing you never sat with. And a human can't hold that much that fast anyway. So nobody holds it. We keep shipping, and then we just ask Claude where the thing we just shipped actually lives.

## Made by Agents, For Agents

Which brings me to what I actually think is happening.

These codebases are made by agents now. Made by agents, for agents. We're not the audience anymore. We're the passenger. The code isn't written for a human to sit down and read. It's written for another agent to pick up and extend, and for you to query when you need something.

Which means you need the AI. Not as a convenience. As the only way in. You can't do without it. For most things, that's just how it is now.

Here's the test I keep running in my head. Tomorrow every model goes dark. Claude, Codex, all of it, gone. How long until someone can work on one of these codebases. Weeks. Months. On some of them, I don't think anyone ever does, because nobody alive held the map in the first place.

And I hate that part. Not the tool. The needing. I hate not knowing my own codebase. I hate that owning it stopped meaning anything.

## There's No Resolution

I keep waiting for the part where it turns around. Where you go back to understanding. Where the map comes back.

I don't think it does. The pace isn't slowing down. This is the future, and we're all just supposed to accept it and move faster than yesterday.

And honestly, I think it's going to raise way more problems than it solves. Way more. It sounds sad but it's real at the same time.

I don't have a fix. I don't think there is one. And that part's disgusting to me.

Anyway. That's where I'm at. Hope I know my own codebase again before I graduate. :))
