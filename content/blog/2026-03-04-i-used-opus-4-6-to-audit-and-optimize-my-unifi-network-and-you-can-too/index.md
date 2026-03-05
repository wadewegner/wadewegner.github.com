---
title: "I Used Opus 4.6 to Audit and Optimize My UniFi Network (And You Can Too)"
date: 2026-03-04T12:00:00-08:00
description: "A step-by-step guide to using Claude Opus 4.6 and the UniFi API to audit your home network, fix radio conflicts, add VLAN segmentation, and harden security — all in about an hour."
draft: false
---

Most of us set up our home networks once and never touch them again. Maybe you ran the UniFi setup wizard, picked an SSID name, and called it a day. Everything works, so why mess with it?

Here's the thing: "works" and "works well" are very different. My network had been running for years with co-channel interference on 2.4GHz, every radio cranked to max power, no VLANs, no intrusion detection, and IoT devices sharing a flat network with my NAS. I knew it could be better but never had the motivation to sit down with the UniFi API docs and figure it all out.

Then I saw this tweet from DHH:

> Giving Opus access to your ubiquity interface to debug wifi and network issues is unbelievably effective. It's now correctly identified and fixed problems in two different installations I've had that were plagued for months/years with issues. So good.
>
> — [@dhh, Mar 2, 2026](https://x.com/dhh/status/2028655013329821845)

One conversation later, my network had clean channel assignments, proper VLAN segmentation, hardened security settings, updated firmware on every device, and a detailed log of every change made. The whole thing took about an hour of wall-clock time, most of which was me reviewing proposed changes and saying "go ahead."

Here's how to do it yourself.

## What You Need

- A **UniFi network**: I'm pretty sure this works with any UniFi setup: a UDM, UDR, or Cloud Key (where the controller is built into the hardware), or a self-hosted UniFi Network Application on Windows/Linux/Raspberry Pi. The API is the same; the only difference is the base path (UDM/UDR uses `/proxy/network/api/s/default/` while self-hosted uses `/api/s/default/`). Don't worry, the LLM will figure it out.
- **Cursor** with a capable model (I used Claude Opus 4.6, but any model with strong tool use should work), or **Claude Code** if you prefer the terminal
- About an hour of your time (less if your network is in better shape than mine was)
- A willingness to let an AI poke at your network config (with your approval at every step)

## Step 1: Set Up API Access

This was the only real tricky part for me. If you log into your UniFi controller with a Ubiquiti SSO account (which most people do), that account uses 2FA and doesn't play nicely with API calls from a script.

The fix: **create a local-only admin account specifically for API access.**

In the UniFi controller UI:
1. Go to **Settings → Admins & Users → Add Admin**
2. Choose **Local Access Only** (not Ubiquiti account)
3. Give it a username and a strong password
4. Set the role to **Administrator** (it needs read/write access)
5. Make sure **Remote Access** is unchecked

Now you have credentials that work with simple username/password authentication against the local API. No 2FA, no SSO token dance.

## Step 2: Write a Context File

This is the key to making the LLM actually useful. Don't just say "optimize my network." Give it a structured handoff document. I created a `context.md` file that included:

**Authentication details:**
```markdown
The UniFi controller is a self-hosted UniFi Network Application running at https://127.0.0.1:8443.

A local admin account was created for API use:
- Username: <your-username>
- Password: <your-password>
- Login endpoint: https://127.0.0.1:8443/api/login

How to authenticate:
1. Write credentials to a file to avoid shell escaping issues
2. curl -k -X POST -c cookies.txt -H "Content-Type: application/json" -d @body.json https://127.0.0.1:8443/api/login
3. Use -b cookies.txt on subsequent requests
```

**API endpoint reference** — a table of every useful endpoint:
```markdown
| Endpoint          | Method | Description                          |
|-------------------|--------|--------------------------------------|
| stat/sysinfo      | GET    | Controller version, system info      |
| stat/device       | GET    | All devices — full detail            |
| stat/sta          | GET    | All connected clients                |
| stat/health       | GET    | Dashboard health                     |
| rest/networkconf  | GET    | Network configs (VLANs, subnets)     |
| rest/wlanconf     | GET    | SSID configs                         |
| rest/firewallrule | GET    | Firewall rules                       |
| cmd/devmgr        | POST   | Device management (provision, etc.)  |
```

**The plan** — what you actually want done:
```markdown
Phase 1: Pull ALL endpoints and save the JSON responses
Phase 2: Analyze and produce recommendations
Phase 3: Apply changes (only after my explicit approval)
```

**Important context** — anything about your setup the LLM needs to know:
```markdown
- This is a self-hosted controller (not UniFi OS), so API base path 
  is /api/s/default/ with no /proxy/network prefix
- Always use -k flag (self-signed cert)
- Port 8443 for management API, port 8080 is device inform only
```

The context file is doing the heavy lifting here. It turns a vague request into a structured task with clear boundaries. The LLM knows how to authenticate, what to look at, and what the rules of engagement are.

And if you're wondering how I built this `context.md` file, that's right, I did it with Opus 4.6.

## Step 3: Start the Conversation

Here's roughly how I kicked it off:

> "I'd like your help optimizing my UniFi network. Context is in context.md."

That's it. The LLM read the context file, authenticated against the API, and started pulling data from every endpoint. Within a few minutes it had a complete picture of my network: hardware inventory, firmware versions, SSID configuration, radio settings, client distribution, RF environment, security posture, everything.

From there I asked it to produce two deliverables:

> "Please write an analysis.md file with your findings and a recommendations.md file with what you suggest we do."

The analysis came back as a structured audit covering topology, RF environment, client health, and security gaps. The recommendations were organized into batches by risk level, each with specific API payloads ready to go.

## Step 4: Work Through the Batches

This is where it gets fun. Rather than applying everything at once, we worked through changes in batches:

**Batch 1: WiFi Radio Optimization** (low risk, high impact)
- Fix channel conflicts
- Widen 5GHz channels
- Adjust transmit power
- Enable fast roaming and multicast enhancement
- Set minimum RSSI thresholds

**Batch 2: Security Hardening** (medium risk)
- Enable protected management frames
- Turn on DNS-over-HTTPS
- Enable DNS filtering

**Batch 3: VLAN Segmentation** (higher risk, critical impact)
- Create an IoT network on its own VLAN
- Create a dedicated IoT SSID
- Set up firewall rules to isolate IoT from trusted devices

**Batch 4: Minor Optimizations** (low risk)
- Enable IGMP snooping
- Disable NAT-PMP

For each batch, the LLM would:
1. Show me exactly what it planned to change and why
2. Wait for my explicit go-ahead
3. Execute the API calls
4. Verify the results
5. Log everything to a change log file

The key prompt pattern was simple:

> "Let's proceed with batch 1. Please create a log.md and track everything you do."

That `log.md` instruction was important. It meant every API call, payload, response, and verification step was recorded. If something went wrong, I could trace exactly what happened and roll it back.

## What We Actually Fixed (Without Getting Too Specific)

Here's a taste of what the LLM found and fixed, generalized so it applies to most UniFi setups:

**Co-channel interference on 2.4GHz.** Two access points were both on channel 1. There are only three non-overlapping 2.4GHz channels (1, 6, 11), and with the controller set to "auto," it had made a bad choice. The LLM assigned manual channels to eliminate overlap and even suggested reducing to three APs since four APs on three channels means at least one conflict.

**5GHz channels stuck at 40MHz width.** The APs supported 80MHz channels (VHT80) but were configured for HT40. Switching to 80MHz nearly doubled available throughput. The LLM also assigned non-overlapping 80MHz blocks across all APs, including DFS channels where appropriate.

**Every radio at max transmit power.** When all your APs are screaming at full volume, clients see strong signals from multiple APs and don't roam efficiently. The LLM lowered 2.4GHz power on most APs to create cleaner cell boundaries, keeping power high only where a distant IoT device needed the extra reach.

**An IoT device that disconnected after optimization.** This is the kind of thing that makes network tuning scary — you change settings and something breaks. The LLM diagnosed it immediately: the minimum RSSI threshold was kicking the device because lowering TX power had pushed its signal just below the threshold. The fix was surgical: relax the threshold slightly and bump power back up on the one AP closest to that device.

**Zero network segmentation.** Every device — laptops, phones, NAS, Ring cameras, smart fridge, sauna controller — was on the same flat network. A compromised IoT device could reach everything. The LLM created an IoT VLAN, a dedicated IoT SSID, and firewall rules that block IoT devices from initiating connections to the trusted network while still allowing your phone to control them.

**Missing security features.** DNS-over-HTTPS was configured but not turned on. DNS filtering was available but disabled. Protected management frames were off. NAT-PMP was still enabled (an attack surface). All easy fixes via the API.

**Firmware updates.** Every device had pending firmware updates. The LLM triggered them all via the API, and when the controller software itself was updated (which I did manually), the devices all showed as "offline" until the LLM force-provisioned them to reconnect.

## Tips and Gotchas

Things I learned that'll save you time:

**PowerShell and JSON don't mix well.** If you're on Windows, don't try to pass JSON directly in curl commands. PowerShell's escaping rules will mangle it. Instead, write your JSON payloads to files and use `curl -d @filename.json`. The LLM figured this out after the first failed API call and switched strategies automatically.

**Some settings silently ignore your values.** For example, setting a minimum data rate required also setting `minrate_setting_preference` to `"manual"` — otherwise the API returned success but didn't actually change anything. The LLM caught this by verifying the config after each change, saw the value hadn't stuck, and figured out the missing parameter.

**"Medium" TX power doesn't always mean medium.** On some AP models, setting `tx_power_mode` to `"medium"` was accepted but the actual transmit power didn't change. Using `"custom"` with an explicit dBm value worked. These are the kinds of API quirks that are painful to discover manually but that an LLM can brute-force through quickly.

**Hardware limitations are real.** We tried to enable WPA3 but the APs were Wave 1 hardware that doesn't support it. We tried to enable IDS but the gateway didn't have the processing power. The LLM identified these limitations from the API responses, logged them, and moved on instead of banging its head against the wall. It also noted them as reasons to consider hardware upgrades down the road.

**Firewall rule indices have a specific valid range.** The API requires rule indices matching a particular regex pattern, and the "obvious" values (2000, 4000) were actually out of range. The LLM discovered the valid pattern from the error response and adjusted. This is the kind of API archaeology that takes a human 30 minutes of frustrated Googling.

**Always verify after firmware upgrades.** Firmware updates can reset settings. After upgrading all devices and the controller software, we did a full verification pass to confirm every configuration change had survived. They all did in our case, but it's worth checking.

## The Prompt Playbook

Here are the prompts that worked well, roughly in order:

1. **Kickoff**: "I'd like your help optimizing my UniFi network. Context is in context.md."
2. **Analysis**: "Please write an analysis.md file with your findings and a recommendations.md file with what you suggest we do."
3. **Execution**: "Let's proceed with batch 1. Please create a log.md and track everything you do."
4. **Course correction**: "What if we just turn off [AP name] and go with three APs? My house isn't that big, and it might help with conflicts." (Don't be afraid to push back or suggest alternatives.)
5. **Troubleshooting**: "I can't get [device] to connect. Can you see what's happening?" (The LLM can pull client stats and diagnose the issue.)
6. **Impact assessment**: "For batch 2, what is the impact to clients and performance?" (Ask before approving risky changes.)
7. **Verification**: "Can you check to confirm the network looks good?"
8. **Next steps**: "Anything else we should do?" (Good for catching things like post-upgrade verification.)

The pattern is: **give context, ask for analysis, review recommendations, approve in batches, verify after each batch, and keep a log.**

## Two Things You Should Absolutely Do

**Download a backup after each batch of changes.** In the UniFi controller UI, go to **Settings → System → Backup** and download a copy of your config. Do this before you start, and again after each batch. If something goes sideways days later and you can't remember what changed, you can restore to any checkpoint. The LLM can also trigger a backup via the API (`POST /api/s/default/cmd/backup`), so you can even bake it into the workflow.

**Disable or delete the local admin account when you're done.** You created it for API access, and now the work is finished. There's no reason to leave an extra admin account sitting around with no 2FA. Either delete it entirely or at minimum disable it. You can always recreate it if you want to run another optimization session later.

## Why This Works

A few reasons this turned out better than I expected:

**The LLM is tireless at API exploration.** It pulled 15+ endpoints, parsed the JSON, cross-referenced device configs with client stats, and identified patterns across dozens of devices. This would take a human an hour just for the data collection.

**It catches things you'd miss.** I didn't know my 5GHz channels were at HT40 when they could be VHT80. I didn't know NAT-PMP was enabled. I didn't know the minimum data rate was set to 1 Mbps. These are settings buried deep in the config that you'd only find by reading every field of every API response.

**It recovers from errors gracefully.** When the PowerShell JSON escaping broke, it switched to file-based payloads. When a setting didn't apply, it investigated why and found the missing parameter. When a device disconnected, it diagnosed the root cause and applied a targeted fix. Each failure became a learning moment that it logged and adapted to.

**The approval-gate pattern keeps you in control.** At no point did the LLM go rogue. Every change was proposed, explained, and waited for my "go ahead." The log file means you have a complete audit trail. And rollback instructions were included for every batch.

**It's reproducible.** The context file, analysis, recommendations, and change log together form a complete record. If you need to redo it (new house, new hardware, friend asks for help), you have a template.

## What It Can't Do

A few limitations worth noting:

- **It can't move devices between SSIDs.** WiFi clients store their own credentials. When you create a new IoT SSID, you have to manually reconnect each device via its companion app or settings screen. There's no API for "tell this Ring camera to use a different network."

- **It can't fix hardware limitations.** If your APs don't support WPA3 or your gateway doesn't have IDS capability, no amount of API calls will change that. But it will tell you exactly what your hardware can and can't do, which is valuable for planning upgrades.

- **It doesn't know your physical layout.** It can tell you that two APs are on the same channel and that's bad, but it doesn't know which APs are physically close together. You'll need to provide that context or correct its assumptions. In my case, I told it which AP was closest to a problematic device, and it adjusted its approach accordingly.

- **Session cookies expire.** Long conversations may need re-authentication partway through. Not a big deal, but worth knowing.

## Get Started

1. Create a local API account on your UniFi controller
2. Write a context file with auth details, endpoint reference, and your goals
3. Open Cursor (or Claude Code) and point it at your context file
4. Let it pull data and analyze
5. Review recommendations, approve in batches
6. Keep a log of everything

The whole approach generalizes beyond UniFi, too. Any system with a REST API — Plex, Home Assistant, Proxmox, your router's admin API — can be audited and optimized the same way. Write a context file, give the LLM credentials and endpoint docs, and let it explore.

Your network is probably fine. But it could be better. And now you have an assistant that'll do the tedious part for you.
