# With Grace for Claude

This connects Claude, the AI assistant at [claude.ai](https://claude.ai), to
your With Grace account. Once it is on, you can ask Claude about your project,
your development or your villa, and it answers from your own records.

It is for With Grace admins, architect studio clients, landowner clients and
villa buyers. [See what each can do](#who-this-is-for).

You need a Claude account, and you only ever see what belongs to you. There is
nothing to download. "MCP" is the name of the technology that links the two.

## How to install With Grace MCP

1. **Add With Grace to Claude.** Open
   [this link](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=With%20Grace&connectorUrl=https%3A%2F%2Fmcp.wgrace.co%2Fmcp)
   while signed in to Claude, and confirm when it asks. A connector is what Claude calls
   an app it is linked to. Only do this step if With Grace has not been added
   before. If you are not sure, go to step 2 first: if With Grace shows up
   there, you can skip this step.
2. **Turn it on.** Open
   [your connectors](https://claude.ai/new#customize/connectors?q=with+grace),
   find With Grace in the list and switch it on. A With Grace sign in page
   opens. Use the email address With Grace has for you.

Then start a new chat in Claude and type a question.
[Here are some to try](#try-it).

If your company has a Claude Team or Enterprise plan, the person who manages it
has to do step 1 once for everyone. After that you only need step 2.

If anything does not work, email [hello@wgrace.co](mailto:hello@wgrace.co) and
we will help you through it.

## Who this is for

What you can do depends on who you are. Claude works it out from the email you
sign in with, so there is nothing to choose.

| You are | What you can ask Claude to do |
| --- | --- |
| **Admin** on the With Grace team | See and update everything across all projects: villas, contacts, campaigns, tasks and owner statements |
| **Architect studio client** who has hired With Grace to design a project | Follow your own project: which stage it is at, what is true now and what happens next |
| **Landowner client** with a development on With Grace | See your own development: which villas are available, reserved or sold, their prices, the people who have enquired, and your latest ad |
| **Buyer** who owns a villa | Read the monthly statements and payouts for your own villa. You can look but not change anything |

If you sign in and Claude finds nothing, your email is probably not linked to a
project or a villa yet. Email [hello@wgrace.co](mailto:hello@wgrace.co) and we
will link it.

## Try it

For a landowner or studio client:

> Where is my project up to, and what happens next?

> Which villas are still available, and what do they cost?

> Add a contact called Mira who asked about the two bedroom villas.

For a buyer:

> Show me last month's statement for my villa.

Claude only reports figures that are written in your records. If a price or a
date has not been recorded, it tells you so.

## What it can reach

Your own records, and only those: your organization, projects, villas, contacts
and ad campaigns. A project is a whole development. A property is one villa or
lot, with its own code, price, area and status.

Some things you can read but not change. Campaigns are made and published by
With Grace, and owner statements are posted by With Grace, so Claude will tell
you it cannot edit those.

## Help

Email [hello@wgrace.co](mailto:hello@wgrace.co). Tell us the email address you
signed in with and what you asked Claude, and we will take it from there.

---

# For technical users

Everything below is optional. If the two steps at the top worked, you are done.

## Other ways to connect

### Paste the address yourself

In Claude on the web, Cowork or Desktop: `Settings`, `Connectors`,
`Add custom connector`, and paste:

```
https://mcp.wgrace.co/mcp
```

Then connect, and sign in when prompted.

On a Team or Enterprise plan an owner adds the connector once for the
organisation, after which each person connects their own account.

For Claude Desktop you can edit the config directly:

```json
{
  "mcpServers": {
    "withgrace": {
      "type": "http",
      "url": "https://mcp.wgrace.co/mcp"
    }
  }
}
```

Restart, then approve the sign in when it prompts.

### Coding agents on your machine

For Claude Code, Codex, Gemini CLI and Cursor, which keep their settings in
files rather than in an account.

```bash
npx withgrace auth login
npx withgrace connect
```

The first prints a short code. Open the link it shows, sign in, and approve.
That sign in is the one step nobody can do on your behalf, because it is your
password. The second finds the coding agents installed on your machine and
writes each one's configuration for you.

It needs Node 20 or newer, and it installs nothing permanently.

Check it worked:

```bash
npx withgrace auth status
```

You want every agent listed as connected. To disconnect everything and end the
credential for good:

```bash
npx withgrace auth logout
```

That revokes on the server first, so the credential is dead even if a copy of a
config file survives somewhere.

Verified with Claude Code, Gemini CLI and Codex. Anything else can take the
block to paste from `npx withgrace connect --print`.

### Or set up one agent by hand

Every agent can also be pointed at the connector directly, which is useful if
you would rather not run anything.

**Claude Code**

```bash
claude mcp add --transport http --scope user withgrace https://mcp.wgrace.co/mcp
claude mcp login withgrace
```

Check with `claude mcp list`. You want a connected marker. Needs authentication
means the sign in has not finished.

Over SSH, add `--no-browser` to the login. It prints a URL to open elsewhere and
waits for you to paste back the address you land on. That needs a real terminal,
so it will not work from inside an assistant. `npx withgrace auth login` has no
such limitation, which is why it is the recommended route.

**Codex**

```bash
codex mcp add withgrace --url https://mcp.wgrace.co/mcp
codex mcp login withgrace
```

Check with `codex mcp get withgrace`.

## Add the skill

The connector gives your assistant the tools. [SKILL.md](SKILL.md) tells it how
to use them well, which mostly means never inventing a price.

For Claude Code, clone this repository into your skills directory:

```bash
git clone https://github.com/With-Grace-Group/skills.git \
  ~/.claude/skills/withgrace
```

For anything else, paste the contents of `SKILL.md` into your assistant's
instructions.

## Managing access

Sign in, open **Settings**, then **Connect**, to see every credential on your
account and revoke any of them. Revocation takes effect on the next request.

A client that signs in through the browser gets its own credential, so revoking
it leaves the others alone. A token you create by hand and paste into several
places is one credential in several files, and revoking it disconnects all of
them at once.

### Connecting something that has no browser

A script or a scheduled job cannot sign in interactively. Sign in once on a
machine that can, then reuse the credential, or create a token under
**Settings**, then **Connect**, and send it as a header:

```
Authorization: Bearer YOUR_TOKEN
```

A token is a credential. It belongs in a config file, not in a shared document,
a screenshot or a repository.

## Troubleshooting

**The assistant says it has no With Grace tools.** The client did not load the
config. Restart it, and check the file is valid JSON or TOML.

**Every call is refused.** The connection was revoked, or the sign in never
finished. Run `npx withgrace auth status` to see which it is, then
`npx withgrace auth login` again.

**`Failed to connect` with an HTTP code.** The client could not reach the
connector at all. Check the URL is exactly `https://mcp.wgrace.co/mcp`.

**`Pending approval`.** The connector was added to a project rather than to
you. Run `claude` and approve it, or add it again with `--scope user`.

**403 when you test the URL with curl.** Almost always your own network, not
ours. An unauthenticated request to the connector answers `401` with a
`WWW-Authenticate` header naming where to sign in. A `403` with no such header
means something between you and us refused the connection, which is common from
inside sandboxed containers.

**A tool returns nothing.** There are no records of that kind for your account
yet. If you expected some, email hello@wgrace.co.

## Support

Email [hello@wgrace.co](mailto:hello@wgrace.co).
