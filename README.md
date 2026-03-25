# Nova
Nova, or Project Nova, is an attempt at making a primarily server sided anti-cheat utilising statistical models to determine a player's legitimacy, and eliminating the need for kernel-level anti-cheats. We may split cheaters into three tiers.

- **Tier 1 (Blatant cheaters):** Spin-botters, 100% Headshot rate etc. A rudamentary surface level anti-cheat should be capable of handling these kinds of cheaters.
- **Tier 2 (Highly suspicious players):** High headshot rate, extreme map awareness and extreme game sense. These cheaters are the ones most competent anti-cheats can catch.
- **Tier 3 (Subtle cheats):** Simply put, these are players that look like very good players and only seasoned players would be able to determine if they cheat or not. Only great anti-cheats catch these players, typically only kernel-level.
- **Tier 4 (Off-machine subtle cheats):** These are the kind of players that look again like very good players, but by using external assitance (on another machine) allows them to bypass any easy detection by kernel-level anticheats, pair that with subtle cheats and these cheaters are practically impossible to catch unless the make a mistake.

Nova aims to be able to deal with all four tiers, but primarily focusing on Tier 2 and 3 cheaters, leaving Tier 1 cheaters to faster acting surface level anti-cheat for swift action. Tier 4 cheaters, while disruptive to gameplay, are a significantly weaker presence and annoyance to a player both due to level of entry of set up and impact on any specific round or match. Thus, these cheaters deal a weaker impact to both the player's experience and the game's health. However, we do wish to catch these players too!


## Development Roadmap Stages

### Stage 1: Nebula
**Goal:** Turn raw, binary ``.dem`` files into readable, structured data using [Awpy](https://github.com/LaihoE/demoparser) and/or [demoparser2](https://github.com/pnxenopoulos/awpy).  
**Approach:**
- Use existing CS:2 demofile parsers and extract relevant data.
  - Define what data is available and what data should be relevant.
- Design a robust SQL database for a large number of rows of data (player coords., view angles, weapon events, utility usage, etc.) per match.
  - A smaller table to house basic metrics for distributions for various ranks and data. (mean and variance/standard deviation)
- [ ] Python demofile parser  
- [ ] SQL database
- [ ] Base set of replays for median rank
- [ ] Parsed data for said set

We should be able to determine a player's surface level "sussyness" based on the $Z$-score. Given by
```math
Z = \frac{x-\mu}{\sigma}
```
Where the $Z$-score determines how many standard deviations $\sigma$ the player's result deviates from the norm.

### Stage 2: Protostar  
**Goal:** Define cheating mathematically for median rank by determining median player behaviour as numerical and statistical models.  
**Approach:** 
- Calculate the time delta between enemy appearing on screen and the player firing.
  - Alternatively when the player "reacts", this could be very hard to define.
- Track crosshair movement, specifically direction and angle changes per tick.
  - A human player has a limit to how "straight" a line could be or how fast a direction can change. Time for complete direction change might not be used due to limited tickrate.
  - Crosshair velocity could be used to catch spinbotters. Very high velocity over a longer period of time which is unsustainable unless sensitivity is very high.
- Account for outliers, a player may do a perfect line flick once and never again in their life.
- [HARD] Determine how often a player directly looks at an enemy without info (audio/visual cues) through walls
- [VERY HARD] Determine how often a player "reacts" based on enemy behaviour without info (audio/visual cues)

During this stage it's imperative to properly define certain behaviours correctly and not "simplify" too much as it could drastically change results further in development. Such as:
*Behaviours to define*:
- [ ] A flick
- [ ] Straightness of a flick
- [ ] Enemy appearing on screen
- [ ] Attempting to fire on enemy and not somewhere/something else
- [ ] Looking at an enemy
- [ ] Looking at an enemy pt.2 (through a wall)
- [ ] More TBD

### Stage 3: Main Sequence
**Goal:** Expand the existing systems for a larger data set including and seperated by ranks (and/or FACEIT level if possible). Larger pools of ranks may be needed for very high and/or very low ranks due to lack of data and players in those ranks.  
**Approach:** The established systems created in stage 1. and 2. should apply directly without need for modifications, only the parameters defining performance should change.
- [ ] Obtain a large amount of demofiles for each rank
- [ ] Parse data from the demofiles
- [ ] Create statistical models for each rank seperation
- [ ] **BOTTLENECK:** C++/Rust implementation of existing models in order to parse data effectively

### Stage 4: Red Giant

### Stage 5: Nova

### Stage 6: Full release (Super Nova)
