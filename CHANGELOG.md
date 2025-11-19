# Changelog

## [Unreleased] - Unity 6000.0.58f1 Upgrade

### Changed
- Updated Unity Editor version from 2017.1.0p3 to 6000.0.58f1
- Updated API compatibility level from .NET 2.0 to .NET Standard 2.1 in ProjectSettings
- Created Packages/manifest.json with Unity 6000 compatible packages:
  - Core packages: TextMeshPro, Timeline, Visual Scripting, URP, Test Framework
  - IDE integrations: Rider, Visual Studio, VS Code
  - Development features and Unity modules
- Fixed deprecated VR API: Changed `UnityEngine.VR.VRSettings` to `UnityEngine.XR.XRSettings` in SkinnerSource.cs

### Known Issues / Manual Steps Required

**IMPORTANT: This upgrade requires manual completion by a maintainer with Unity Editor access.**

#### Required Manual Steps:

1. **Install Unity 6000.0.58f1**
   - Download and install Unity Hub
   - Install Unity Editor version 6000.0.58f1 through Unity Hub
   - Ensure all required build platform modules are installed

2. **Open Project in Unity 6000**
   - Open the project in Unity 6000.0.58f1
   - Unity will automatically upgrade project assets and reimport all assets
   - This process may take several minutes depending on project size
   - Review any console warnings or errors during asset upgrade

3. **Verify Shader Compilation**
   - All shaders in Assets/Skinner/Shader/ will be recompiled by Unity
   - Check for any shader compilation errors or warnings
   - Test shader functionality in sample scenes

4. **Update PostProcessing Package**
   - The included PostProcessing package in Assets/PostProcessing is a legacy asset store package
   - Consider migrating to Unity's built-in Post Processing Stack v2 or URP post-processing
   - Or update the legacy package to be compatible with Unity 6000 if errors occur

5. **Test Runtime Functionality**
   - Open test scenes in Assets/Test/
   - Enter Play mode and verify all Skinner components work correctly
   - Test SkinnerSource, SkinnerParticle, SkinnerTrail, SkinnerGlitch, and SkinnerDebug components
   - Verify vertex baking, particle emission, and trail rendering

6. **Run Editor Tests**
   - Test the Skinner Model conversion tool (Assets menu → Skinner → Convert Mesh)
   - Verify mesh conversion works for skinned meshes
   - Check that Skinner templates (particle, trail, glitch) can be created/edited

7. **Build and Platform Testing**
   - Test builds for target platforms (Windows, macOS, iOS, etc.)
   - Verify Metal graphics API support (iOS/macOS)
   - Test on target hardware if possible

8. **Performance Validation**
   - Profile the application to ensure no performance regressions
   - Check GPU usage with Profiler
   - Verify render texture creation and MRT rendering still works efficiently

### Code Changes Requiring Review

1. **BoneWeights API (SkinnerModel.cs)**
   - Current code uses deprecated `mesh.boneWeights` property
   - Unity 2019.1+ introduced new API: `GetAllBoneWeights()` and `SetBoneWeights()`
   - New API supports more than 4 bone weights per vertex
   - TODO: Evaluate migration to new API for better performance
   - Current implementation will work but may show deprecation warnings

2. **Mesh Upload (SkinnerModel.cs)**
   - Code uses `mesh.UploadMeshData(true)` which marks mesh as no-longer-readable
   - Verify this behavior is still desired in Unity 6000

3. **VR/XR Support (SkinnerSource.cs)**
   - Updated from deprecated `UnityEngine.VR.VRSettings` to `UnityEngine.XR.XRSettings`
   - Test with XR/VR devices if VR support is required

### Compatibility Notes

- **Graphics APIs**: Project should work with Unity 6000's supported graphics APIs (Metal, Vulkan, DirectX 11/12)
- **Platforms**: Compatibility should be maintained for Windows, macOS, iOS (Metal)
- **Shaders**: All shaders use standard CG/HLSL syntax compatible with Unity 6000
- **Build Pipeline**: No changes needed for build pipeline
- **Package Manager**: Now using Unity Package Manager (Packages/manifest.json) instead of only Asset Store packages

### Testing Performed

- Code review for deprecated API usage
- Static analysis for Unity 2017 to Unity 6000 API changes
- Fixed known deprecated APIs (VR namespace)
- Added TODO comments for potential improvements

### Not Tested (Requires Unity Editor)

- Asset database upgrade
- Shader compilation
- Runtime execution
- Play mode tests
- Editor tools functionality
- Platform builds
- Performance profiling

### References

- Unity 6000 Release Notes: https://unity.com/releases/editor/whats-new/6000.0.0
- Unity Upgrade Guide: https://docs.unity3d.com/Manual/UpgradeGuides.html
- API Changes: https://docs.unity3d.com/6000.0/Documentation/ScriptReference/
