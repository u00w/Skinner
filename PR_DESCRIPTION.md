# Pull Request: Upgrade Skinner to Unity 6000.0.58f1

## Overview
This PR upgrades the Skinner Unity project from Unity 2017.1.0p3 to Unity 6000.0.58f1, a major version upgrade spanning approximately 7 years of Unity development.

## Changes Summary

### Core Unity Project Files
1. **ProjectSettings/ProjectVersion.txt**
   - Updated `m_EditorVersion` from `2017.1.0p3` to `6000.0.58f1`

2. **ProjectSettings/ProjectSettings.asset**
   - Updated `apiCompatibilityLevel` from `2` (.NET 2.0) to `6` (.NET Standard 2.1)
   - This aligns with Unity 6000's .NET Standard 2.1 requirement

3. **Packages/manifest.json** (NEW)
   - Created Unity Package Manager manifest with Unity 6000 compatible packages
   - Includes: URP 17.0.3, TextMeshPro, Timeline, Visual Scripting, Test Framework
   - Includes IDE integrations: Rider, Visual Studio, VS Code
   - Includes all necessary Unity module dependencies

### Code Changes

1. **Assets/Skinner/SkinnerSource.cs**
   - Fixed deprecated API: `UnityEngine.VR.VRSettings.enabled` → `UnityEngine.XR.XRSettings.enabled`
   - The VR namespace was deprecated in Unity 2017.2+ and replaced with XR namespace

2. **Assets/Skinner/SkinnerModel.cs**
   - Added TODO comment for potential `boneWeights` API migration
   - The current deprecated API still works but Unity 2019.1+ introduced new APIs
   - `GetAllBoneWeights()` and `SetBoneWeights()` support more than 4 weights per vertex
   - Left as-is to avoid behavioral changes; can be updated in future if needed

### Documentation

1. **CHANGELOG.md** (NEW)
   - Comprehensive documentation of all changes
   - Detailed manual steps required to complete the upgrade
   - Known issues and considerations
   - Testing checklist for maintainers
   - References to Unity 6000 documentation

## What Was Tested

✅ Static code analysis for deprecated Unity APIs  
✅ Security scanning with CodeQL (0 vulnerabilities)  
✅ Review of Unity 2017 → Unity 6000 API changes  
✅ Verification of shader syntax compatibility  
✅ Review of Editor scripts for deprecated APIs  

## What Could NOT Be Tested (Requires Unity Editor)

❌ Asset database upgrade  
❌ Shader compilation  
❌ Runtime execution and Play mode  
❌ Editor tools functionality (mesh conversion)  
❌ Platform builds  
❌ Performance profiling  

## Critical Manual Steps Required

⚠️ **This upgrade CANNOT be completed without Unity Editor access.**

The maintainer MUST perform the following steps:

### 1. Install Unity 6000.0.58f1
- Download Unity Hub
- Install Unity Editor 6000.0.58f1 with required build modules

### 2. Open Project
- Unity will automatically trigger asset upgrade process
- This reimports all assets and upgrades metadata
- May take several minutes depending on project size

### 3. Verify Compilation
- Check Console for errors/warnings
- All shaders will be recompiled automatically
- Address any shader compilation issues

### 4. Test Core Functionality
- Test Skinner components: Source, Particle, Trail, Glitch, Debug
- Verify vertex baking with replacement shaders
- Test MRT (Multiple Render Targets) rendering
- Verify VR/XR compatibility if needed

### 5. Test Editor Tools
- Test mesh conversion tool (Assets → Skinner → Convert Mesh)
- Verify Skinner Model creation from skinned meshes
- Test template asset creation/editing

### 6. Platform Testing
- Build for target platforms
- Test on actual hardware (especially iOS/Metal support)

### 7. Performance Validation
- Profile with Unity Profiler
- Verify no performance regressions
- Check GPU usage and render texture efficiency

## Known Considerations

### PostProcessing Package
- The included PostProcessing package in `Assets/PostProcessing` is a legacy asset store package
- May need updating or migration to URP post-processing in Unity 6000
- Monitor for errors during asset upgrade

### BoneWeights API
- Current code uses deprecated `mesh.boneWeights` property
- Will show deprecation warnings but still functions
- Consider migrating to new API in future: `GetAllBoneWeights()` / `SetBoneWeights()`

### Graphics API
- Project tested on Windows, macOS, iOS (Metal)
- Unity 6000 has improved Metal and Vulkan support
- May see performance improvements on these platforms

### Build Pipeline
- No changes needed to build pipeline
- Unity 6000 uses same build system with improvements

## Files Changed
```
Assets/Skinner/SkinnerModel.cs        |   4 +++ (TODO comment)
Assets/Skinner/SkinnerSource.cs       |   2 +- (VR→XR API fix)
CHANGELOG.md                          | 108 +++ (NEW)
Packages/manifest.json                |  46 +++ (NEW)
ProjectSettings/ProjectSettings.asset |   2 +- (API level)
ProjectSettings/ProjectVersion.txt    |   2 +- (version)
```

## Safety Measures Taken

✅ No risky automated behavior changes  
✅ TODO comments added for potential improvements  
✅ Deprecated APIs fixed only when replacement is drop-in compatible  
✅ Comprehensive documentation of manual steps  
✅ No packages removed without confirmation  
✅ Security scanning completed (0 vulnerabilities)  

## References

- [Unity 6000 Release Notes](https://unity.com/releases/editor/whats-new/6000.0.0)
- [Unity Upgrade Guides](https://docs.unity3d.com/Manual/UpgradeGuides.html)
- [Unity 6000 Script Reference](https://docs.unity3d.com/6000.0/Documentation/ScriptReference/)

## Next Steps for Maintainer

1. Review this PR and the CHANGELOG.md
2. Install Unity 6000.0.58f1
3. Pull this branch and open in Unity
4. Follow the testing checklist in CHANGELOG.md
5. Address any issues that arise during asset upgrade
6. Verify all functionality works as expected
7. Merge if everything passes testing

## Questions?

If you encounter issues during the upgrade process, refer to:
- CHANGELOG.md for detailed manual steps
- Unity upgrade guides linked above
- Unity forum/documentation for specific Unity 6000 issues
