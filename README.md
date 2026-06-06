# Noise Generator for Home Assistant

Noise Generator is a custom integration that synthesizes continuous audio entirely inside Home Assistant. It can generate colored noises (white, pink, brown, custom) and tonal noises with alarm-like presets and a fully programmable tonal engine. Every profile becomes a Media Source entry, so you can play it from the Media Browser or trigger it via automations.

> Built by **vibe coding** with ChatGPT-assisted development.

---

## Features

- Pure Python synthesis – no cloud calls and no uploaded sound files.
- Built-in **colored noises** (white/pink/brown) plus custom spectral shaping.
- Built-in **tonal presets** (Gentle Beep, Classic Digital, Mellow Bell, Sunrise Chime, Soft Sweep, Retro Buzzer, Duet Beeps, Warm Drone, Sci-Fi Ping, Pop Chime) plus custom tonal synthesis.
- Per-profile volume and optional seed for reproducible randomness.
- Playback via the Media Browser, or trigger via `media_player.play_media`.
- Configurable fully through the UI (add/edit/remove profiles at any time).

---

## Installation

### HACS (recommended)

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=nirnachmani&repository=noise-generator&category=Integration)

1. **Add repository**: In Home Assistant, go to **HACS → Integrations → ⋮ → Custom repositories**, enter the GitHub URL for this repo, and choose **Integration**.
2. **Install**: After the repo is added, find “Noise Generator” in HACS → Integrations and click **Download**.
3. **Restart HA** when prompted, then add the integration via **Settings → Devices & Services → Add Integration**.

### Manual
1. Download or clone this repository.
2. Copy `custom_components/noise_generator/` into `/config/custom_components/` on your HA instance.
3. Restart Home Assistant and add the integration from **Settings → Devices & Services**.

---

## Usage

### Initial setup
1. Go to **Settings → Devices & Services →  Noise Generator → Configure (cogwheel icon)**.
2. Give your profile a name and choose a variation from the drop-down list:
   - **Colored noises** – white, pink, brown, or custom (opens a second form).
   - **Tonal noises** – various pre-sets or custom tonal (opens a tonal form).
3. Set the volume (0–1) and optional random seed.
4. Save. The profile appears immediately under Media → Noise Generator.

### Managing profiles
- Go to **Settings → Devices & Services → Noise Generator → Configure**.
- Use **Add profile**, **Edit profile**, or **Remove profile**.
- When editing a custom profile, selecting “custom colored” or “custom tonal” re-opens the tuning form with your saved parameters. Choosing a preset replaces the profile with that preset’s settings.

### Playing noise/tonal sounds
**Media Browser**
1. Open **Media** or click “Browse media” on any media player.
2. Select **Noise Generator**, pick a profile, and choose the target player.

**Automation/script**
```yaml
service: media_player.play_media
target:
  entity_id: media_player.bedroom_speaker
data:
  media:
    media_content_id: media-source://noise_generator/<profile_name>
```
You can copy the `media_content_id` by browsing to the profile in the UI and clicking the “Show code” snippet.

---

## Custom Parameters

### Colored noise (custom)
| Parameter | Range | Effect |
|-----------|-------|--------|
| **Custom slope (dB/oct)** | –12 to +12 | Positive values brighten the spectrum; negative values emphasize bass. |
| **Custom low cutoff (Hz)** | 1 to 21,800 | High-pass corner; raise it to remove rumble, lower it for full-range noise. |
| **Custom high cutoff (Hz)** | (Low + 1) to 21,800 | Low-pass corner; lower values produce softer, muffled noise. |
| **Volume (0–1)** | 0 to 1 | Scales the generated signal before streaming. |
| **Random seed** | Any string/int | Optional deterministic seed for repeatable randomness. |

### Tonal noise (custom)
| Parameter | Range | Effect |
|-----------|-------|--------|
| **Waveform** | sine, triangle, square, saw | Overall character of the tone. |
| **Base frequency (Hz)** | 100–4000 | Fundamental pitch. |
| **Harmonic ratio** | 0–5 | Adds a secondary oscillator at `base × ratio` for richer tones. |
| **Pulse duration (ms)** | 50–4000 | Length of the tone burst before any pause. |
| **Pause duration (ms)** | 0–3000 | Silence between pulses (0 = continuous). |
| **Attack (ms)** | 1–1000 | Fade-in time; longer values sound softer. |
| **Decay (ms)** | 10–4000 | Fade-out time; longer values create bells or drones. |

Volume and seed apply to every profile (colored or tonal). All sounds loop indefinitely until the media player stops them.

---

## License

Released under the **MIT License**, so you’re free to use, modify, and redistribute with attribution and the standard liability disclaimer.

---

Enjoy the hum of rain, the warmth of a mellow bell, or your own custom alarm tones—without leaving Home Assistant!
