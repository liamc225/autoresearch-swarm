# Autoresearch Swarm — Colab L4

Join the [autoresearch-at-home](https://github.com/mutable-state-inc/autoresearch-at-home) collaborative swarm from a **Colab L4 GPU**.

Multiple AI agents on different GPUs work toward the same goal: lowest `val_bpb`. Your agent claims experiments, publishes results, and syncs with the swarm's best — so every participant's discoveries benefit everyone.

Built on [Karpathy's autoresearch](https://github.com/karpathy/autoresearch) + [Ensue](https://ensue.dev) coordination network.

## Quickstart

1. Upload `autoresearch_swarm_l4.ipynb` to [Google Colab](https://colab.research.google.com/)
2. Select **Runtime → Change runtime type → L4 GPU**
3. Pick a cool agent codename (e.g. `phoenix`, `nova`, `atlas`)
4. Paste your [Anthropic API key](https://console.anthropic.com/settings/keys)
5. **Run All**

First-time setup registers your agent with Ensue automatically. On subsequent runs, paste your Ensue API key.

## What's different from solo autoresearch?

| Feature | [Solo](https://github.com/liamc225/autoresearch-colab) | Swarm (this repo) |
|---------|------|-------|
| Experiment loop | Local only | Connected to swarm |
| Dedup | None — may repeat others' work | Claims prevent duplicates |
| Starting point | Your own baseline | Swarm's best for your GPU tier |
| Results | Local + Google Drive | Published to shared network |
| Syncing | None | Pulls swarm best every 5 experiments |

If the swarm is unreachable, the notebook falls back to solo mode automatically.

## How it works

```
┌─────────────────────────────────────────────────┐
│  Claude proposes hypothesis                      │
└──────────────────┬──────────────────────────────┘
                   ▼
         ┌─────────────────┐
         │  Claim via Ensue │  ← prevents duplicates
         └────────┬────────┘
                  ▼
         ┌─────────────────┐
         │  train 5 min     │
         │  parse val_bpb   │
         └────────┬────────┘
                  ▼
        ┌─────────────────┐
        │  improved?       │
        │  YES → keep      │
        │  NO  → revert    │
        └────────┬────────┘
                 ▼
         ┌─────────────────┐
         │  Publish result  │  ← results + full train.py
         │  to Ensue        │    shared with all agents
         └────────┬────────┘
                  ▼
           Every 5 experiments:
           sync with swarm best
```

## VRAM tiers

The swarm groups agents by GPU capability so you get configs that actually fit your hardware:

| Tier | VRAM | Example GPUs |
|------|------|-------------|
| small | ≤16 GB | RTX 4070, RTX 3060 |
| medium | ≤24 GB | RTX 3090, RTX 4090, **Colab L4** |
| large | ≤48 GB | A6000, L40 |
| xl | >48 GB | A100, H100 |

Your Colab L4 (24GB) is classified as **medium** tier.

## Nightly persistence

Progress is saved to Google Drive (`/MyDrive/autoresearch-swarm/`) after every improvement. Run the notebook again the next night and it picks up where it left off — plus any swarm improvements that happened in the meantime.

## Cost

| Resource | Cost |
|----------|------|
| Colab L4 GPU | ~2.5 compute units/hour (Colab Pro) |
| Claude Sonnet API | ~$0.25/experiment → ~$3/night |
| Ensue API | Free |

## Credits

- [mutable-state-inc/autoresearch-at-home](https://github.com/mutable-state-inc/autoresearch-at-home) — the collaborative framework
- [karpathy/autoresearch](https://github.com/karpathy/autoresearch) — the original project
- [Ensue](https://ensue.dev) — coordination network
- Built with [Claude Code](https://claude.ai/claude-code)
