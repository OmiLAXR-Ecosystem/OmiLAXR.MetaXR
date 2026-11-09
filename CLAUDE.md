# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Working rules (versioning, CHANGELOG, `.meta` files, licenses, compatibility) are in @AGENTS.md. Follow them.

## What this is

`com.rwth.unity.omilaxr.metaxr`: Adapter module that tracks Meta XR SDK components (Interaction SDK interactables, teleport, `OVRInput`, `OVRHand`, `OVREyeGaze`, face tracking) in an OmiLAXR learner pipeline.

## Commands

There is no command-line build or linter: the package compiles inside a Unity project. Paths below are relative to the workspace root (`..`).

- **Develop:** `OmiLAXR_Dev/` (Unity 6000.3.13f1) does not link this package. Add `"com.rwth.unity.omilaxr.metaxr": "file:<abs path to OmiLAXR.MetaXR>"` to `OmiLAXR_Dev/Packages/manifest.json`, together with any packages its asmdef references.
- **Tests:** none. There is no `Tests/` folder, and the required SDK is not available locally.

## Architecture

- **Listeners** (`Runtime/Listeners`): mostly `AutoListener<T>` for Interaction SDK types (`GrabInteractable`, `HandGrabInteractable`, `RayInteractable`, `InteractableUnityEventWrapper`, `PointableUnityEventWrapper`). There are also a teleport listener, an `OVRHand` transform listener, and a `GazeDetectorListener<OVREyeGaze>`.
- **Tracking behaviours** (`Runtime/TrackingBehaviours`) are `sealed` subclasses of core abstract behaviours: `InteractableTrackingBehaviour`, `TeleportationTrackingBehaviour`, `InputSystemTrackingBehaviour`, `EyeGazeTrackingBehaviour` and `FacialTrackingBehaviour`. The `OmiLAXR.xAPI` composers handle them.
- **Context and components**: `MetaXRSdkProvider` (core `SdkProvider`) reports the SDK in statements. `MetaXREyeCalibrator` and `MetaXROVRFaceData` adapt eye calibration and face expressions to core components.
- **Prefab**: `Resources/Prefabs/OmiLAXR (MetaXR Variant).prefab` (a variant of the core prefab).
