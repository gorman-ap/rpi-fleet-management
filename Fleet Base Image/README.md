# Fleet Base Image (Abandoned Project)

## Overview

This folder was originally created to support the development of a custom base image for Raspberry Pi devices running **PiSignage** in our production signage environment. The goal was to build a self-maintained image compatible with **Raspberry Pi 5**, due to the official PiSignage OS lacking support for newer hardware.

## Why We Started

- The fleet required **consistent remote deployment** of PiSignage displays across multiple locations, including out-of-state facilities.
- The official PiSignage OS did not support the Raspberry Pi 5 at the time of testing.
- We intended to create a custom OS by modifying **Raspberry Pi OS Lite (Bookworm)**, then manually integrating the core PiSignage components, configuring kiosk mode, and adding system monitoring tools like **Node Exporter**.

## What We Tried

- Rebuilt the signage environment manually using **Raspberry Pi OS Lite** as a base.
- Migrated player files and created a systemd service for `pisignage.js`.
- Installed dependencies such as X11, Chromium, and attempted to start the player dashboard at boot.
- Connected to the PiSignage portal and verified licensing and device recognition.
- Integrated monitoring tools for potential expansion into centralized dashboard management (Prometheus + Grafana).

## Why It Was Abandoned

Despite early progress, several critical issues led to the decision to halt the project:

- **Persistent display failures** due to missing or unstable GUI support on Pi 5.
- **Browser startup issues** with Chromium in kiosk mode from custom setups.
- Even with successful portal registration, the signage player remained **offline or unresponsive**.
- The return on investment didn’t justify the long-term maintenance effort for each Pi image.

## Current Approach

We have returned to using **official PiSignage OS images** and are holding off on deploying Raspberry Pi 5 devices for signage until official support becomes available.

Monitoring, remote access, and security configurations are now handled using **Ansible**, **Node Exporter**, and **Grafana**, separate from the signage layer.

## Status

**This project is no longer maintained and is archived as a record of the attempt.**
