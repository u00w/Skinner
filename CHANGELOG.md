# Changelog

## Unity 6000.0.58f2 Upgrade

### Changes Made

This upgrade migrates the Skinner project from Unity 2017.1.0p3 to Unity 6000.0.58f2 - a major version upgrade spanning 7+ years of Unity evolution.

#### Project Configuration

1. **Unity Version**
   - Updated `ProjectSettings/ProjectVersion.txt` from Unity 2017.1.0p3 to 6000.0.58f2

2. **Package Manager**
   - Created `Packages/manifest.json` with Unity 6000 compatible package versions:
     - com.unity.collab-proxy: 2.5.2
     - com.unity.feature.development: 1.0.3
     - com.unity.textmeshpro: 3.2.0-pre.12
     - com.unity.timeline: 1.8.7
     - com.unity.ugui: 2.0.0
     - com.unity.visualscripting: 1.9.4
     - All Unity built-in module packages
   - Created `Packages/packages-lock.json` for package resolution

3. **Assembly Definitions**
   - Created `Assets/Skinner/Skinner.Runtime.asmdef` for runtime code organization
   - Created `Assets/Skinner/Editor/Skinner.Editor.asmdef` for editor code organization
   - These improve compilation times and enable better code organization

4. **Scripting Settings**
   - Updated `apiCompatibilityLevel` from .NET 2.0 (value: 2) to .NET Standard 2.1 (value: 6)
   - Updated `scriptingRuntimeVersion` from Legacy (value: 0) to Latest (value: 1)

#### Code Changes

1. **VR/XR API Compatibility** (`Assets/Skinner/SkinnerSource.cs`)
   - Replaced deprecated `UnityEngine.VR.VRSettings.enabled` with conditional compilation
   - Added support for `UnityEngine.XR.XRSettings` for Unity 2019.3+
   - Added TODO comment to verify XR compatibility with Unity 6000 XR systems
   - This maintains backwards compatibility while supporting modern XR frameworks

### What Was Tested

- Static code analysis for deprecated API usage
- Assembly definition file validation
- Package manifest JSON structure validation

### What Could NOT Be Completed Automatically

The following require the Unity 6000.0.58f2 Editor to be installed:

1. **Asset Database Upgrade**
   - Unity needs to upgrade the asset database format
   - Reimport all assets with new serialization format
   - Shader compilation for new graphics APIs

2. **Post-Processing Stack**
   - The project uses an older Post Processing Stack that may need updating
   - Located in `Assets/PostProcessing/`
   - May need to migrate to Post Processing Stack v2 or URP/HDRP volumes

3. **Graphics API Compatibility**
   - Shaders in `Assets/Skinner/Shader/` need recompilation
   - Verify shader compatibility with Unity 6000 graphics APIs
   - Test render pipeline compatibility

4. **Runtime Testing**
   - Play mode tests need to be run in Unity Editor
   - Test all Skinner components (Source, Particle, Trail, Glitch, Debug)
   - Verify rendering output matches expected behavior
   - Test VR/XR compatibility if applicable

5. **Build Testing**
   - Test builds for target platforms (Windows, macOS, iOS, etc.)
   - Verify no runtime errors in standalone builds

### Manual Steps Required

To complete the Unity 6000 upgrade, the maintainer must:

1. **Install Unity 6000.0.58f2**
   - Download and install Unity 6000.0.58f2 from Unity Hub

2. **Open the Project**
   ```bash
   # Open the project in Unity 6000.0.58f2
   # Unity will automatically prompt to upgrade the project
   ```

3. **Asset Upgrade Process**
   - Unity will upgrade the asset database - this may take several minutes
   - Review any warnings or errors in the Console
   - Reimport all assets if needed: Assets → Reimport All

4. **Verify Compilation**
   - Check the Console for any compilation errors
   - Fix any remaining API compatibility issues

5. **Test Rendering**
   - Open test scenes in `Assets/Test/`
   - Enter Play mode and verify Skinner effects render correctly
   - Check for visual regressions or errors

6. **Update Post Processing (if needed)**
   - The Post Processing Stack may need updating
   - Consider migrating to Unity's built-in post-processing or URP

7. **Run Tests**
   - Run any existing play mode tests
   - Create new tests if needed to validate functionality

8. **Test Builds**
   - Create test builds for target platforms
   - Verify runtime behavior matches expected results

9. **Update Documentation**
   - Update README.md with Unity 6000 compatibility notes
   - Document any known issues or breaking changes

### Known Issues and TODOs

1. **XR/VR Compatibility**
   - The XR compatibility fix uses conditional compilation
   - Need to verify with Unity 6000's XR plugin framework
   - May need to add XR plugin packages if VR support is required

2. **Post Processing Stack**
   - Current version may be outdated
   - Consider upgrading to Post Processing Stack v2 or migrating to URP

3. **Shader Compatibility**
   - All shaders need to be tested in Unity Editor
   - May need updates for new shader compilation or graphics APIs

4. **Legacy Code Patterns**
   - Some code patterns may benefit from modernization
   - Consider using newer Unity APIs where appropriate

### Breaking Changes

- Minimum Unity version is now 6000.0.58f2
- Projects using Unity 2017-2020 cannot open this upgraded version
- .NET 2.0 code is no longer supported - upgraded to .NET Standard 2.1

### Rollback Instructions

If the upgrade causes issues, you can rollback by:

```bash
git checkout <previous-branch>
```

However, once assets are upgraded in Unity 6000, they cannot be downgraded to Unity 2017 without re-importing from original sources.

### References

- [Unity 6000 Release Notes](https://unity.com/releases/editor/whats-new/6000.0.0)
- [Unity Scripting API Changes](https://docs.unity3d.com/2023.3/Documentation/Manual/UpgradeGuides.html)
- [Package Manager Documentation](https://docs.unity3d.com/Manual/Packages.html)
- [Assembly Definitions](https://docs.unity3d.com/Manual/ScriptCompilationAssemblyDefinitionFiles.html)
