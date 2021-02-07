# Pasuuna Module Player

Module player with support for SoundTracker, ProTracker and FastTracker formats. See basic demo of it in action: https://rorkien.github.io/pasuuna-player

Based on [BassoonTracker](https://github.com/steffest/BassoonTracker) by @Steffest

# Goals

The goal of this project is to rewrite the player portion, streamline the playback API and add a better eventing system.

This will then be used for Phaser 3 and other browser games with additional plugins.

Still a work in progress, will create proper demos once we're there.

# Fork details

This fork adds seeking capabilities to the tracker. This enables a developer to specify which pattern should be playing at any given time. This fork also fixes some variable inconsistencies and typos.

## Pattern numbers and indexes

In a module file, there are two ways to refer to a specific pattern: By their indexes or their numbers.

Pattern numbers are specified by the song's author and can appear multiple times in a song: 

`58, 1, 3, 4, 5, 58, 0`

Indexes, on the other way, specify which order the patterns should be played:

`0, 1, 2, 3, 4, 5, 6`

So, the pattern of index `0` refers to the pattern of number `58` (the 1st item on the pattern array), not the pattern of number `0` (the 7th item on the pattern array).

## Playing a module

```javascript
const tracker = new PasuunaPlayer.Tracker();
tracker.load(url);
tracker.playSong(/*patternIndex*/);
```

The `patternIndex` argument is optional; if unspecified, the song will play from the beggining.

## Seeking a pattern on a playing module

```javascript
tracker.setPattern(/*patternIndex*/, /*resetStep*/, /*stopNotes*/);
```

or

```javascript
tracker.setPatternFromNumber(/*patternNumber*/, /*resetStep*/, /*stopNotes*/);
```

The `resetStep` argument is optional and defaults to `true`. If `true`, the player will start playing the specified pattern from the beggining. If `false`, the player will start playing from the previous pattern's step number. This is useful if the song's patterns beat-match, so skipping will create interesting effects.

The `stopNotes` argument is also optional and defaults to `false`. If `true`, the player will stop any playing notes before skipping to the specified pattern.

# Notes
* There is a small delay between the playback events firing and playback itself. You can find the delay in the enum.js/SETTINGS constants. It's probably set to about 0.05. If you don't need accuracy, you can increase this. It may improve cpu usage/compatibility. But on higher delays, you can't for example track pattern changes or notes playing.