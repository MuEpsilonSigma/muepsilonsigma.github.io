---
title: Projects - Joon
layout: page_with_tabs
permalink: /members/joon/projects/
tabs:
  - title: About
    url: /members/joon/
  - title: Projects
    url: /members/joon/projects/
  - title: Publications
    url: /members/joon/publications/
---

## Current Projects

#### Capacitive sensor array for in-situ measurement of frost & condensate accumulation on cold surfaces

* Condensation frosting demonstration

<div class="paired-video-grid">
  <video id="condensation-video-reference" autoplay muted playsinline loop>
    <source src="{{ '/members/joon/media/20260320_condensation_test_rec_synced_60x_trimmed.mp4' | relative_url }}" type="video/mp4">
  </video>
  <video id="condensation-video-target" autoplay muted playsinline loop>
    <source src="{{ '/members/joon/media/20260320_condensation_test_synced_60x_trimmed.mp4' | relative_url }}" type="video/mp4">
  </video>
</div>

* Random droplet deposition demonstration

<div class="paired-video-grid">
  <video id="droplets-video-reference" autoplay muted playsinline loop>
    <source src="{{ '/members/joon/media/random_droplets_rec_4x_synced.mp4' | relative_url }}" type="video/mp4">
  </video>
  <video id="droplets-video-target" autoplay muted playsinline loop>
    <source src="{{ '/members/joon/media/random_droplets_4x.mp4' | relative_url }}" type="video/mp4">
  </video>
</div>

* Droplet size & phase transition sensing capabilites

<div style="text-align: center;">
  <img src="{{ '/members/joon/media/proposal_figure_2.png' | relative_url }}" alt="proposal_figure_2" />
</div>

## Previous Projects


<style>
  .paired-video-grid {
    display: grid;
    grid-template-columns: auto auto;
    justify-items: center;
    gap: 0.5rem;
    margin: 1rem 0 1rem;
  }

  .paired-video-grid video {
    width: auto;
    height: 270px;
  }

  @media (max-width: 900px) {
    .paired-video-grid {
    grid-template-columns: 1fr;
    justify-items: center;
    }

    .paired-video-grid video {
      height: auto;
      width: clamp(0px, 80vw, 360px);
    }
  }
</style>

<script>
  (function () {
    const DRIFT_TOLERANCE_SECONDS = 0.12;

    const setupSyncedPair = (masterId, slaveId) => {
      const master = document.getElementById(masterId);
      const slave = document.getElementById(slaveId);

      if (!master || !slave) return;

      const syncToMaster = () => {
        if (!isFinite(master.currentTime)) return;
        if (Math.abs(master.currentTime - slave.currentTime) > DRIFT_TOLERANCE_SECONDS) {
          slave.currentTime = master.currentTime;
        }
      };

      const tryAutoplay = async () => {
        try {
          await Promise.all([master.play(), slave.play()]);
        } catch (e) {
          // Autoplay may fail in restrictive browser settings.
        }
      };

      master.addEventListener("play", () => {
        syncToMaster();
        slave.play().catch(() => {});
      });
      master.addEventListener("pause", () => slave.pause());
      master.addEventListener("seeking", syncToMaster);
      master.addEventListener("seeked", syncToMaster);
      master.addEventListener("ratechange", () => {
        slave.playbackRate = master.playbackRate;
      });
      master.addEventListener("volumechange", () => {
        slave.muted = master.muted;
        slave.volume = master.volume;
      });
      master.addEventListener("ended", () => {
        slave.currentTime = 0;
        slave.play().catch(() => {});
      });

      master.addEventListener("loadedmetadata", () => {
        slave.currentTime = master.currentTime || 0;
        tryAutoplay();
      });

      setInterval(() => {
        if (!master.paused && !slave.paused) syncToMaster();
      }, 250);
    };

    setupSyncedPair("condensation-video-reference", "condensation-video-target");
    setupSyncedPair("droplets-video-reference", "droplets-video-target");
  })();
</script>