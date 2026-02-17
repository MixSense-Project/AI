# AI_recommend — MixSense Recommendation System

## Overview

This module implements:

### 1. Home Feed (18 Tracks)
- Buckets: Repeat / LikeSimilar / Discovery
- Global constraint: **max 1 track per artist across all 18**

### 2. Autoplay (Next-Track + Fallback)
- 1st-order Markov model trained from Spotify StreamingHistory logs
- Gating rule:
  - if outgoing transitions ≥ 5 → MARKOV_FIRST
  - else → FALLBACK_FIRST
- Artist constraint: **max 1 track per artist in Top10**

---

## Folder Structure

